<template>
  <div class="upload-container">
    <!-- Team Dropdown -->
    <div class="team-selection">
      <p class="team-text">Team</p>
      <select v-model="selectedTeam" class="team-dropdown">
        <option v-for="(team, index) in teams" :key="index" :value="team">{{ team }}</option>
      </select>
    </div>

    <!-- Upload Area -->
    <div class="upload-main">
      <!-- Upload Box on Left -->
      <div class="upload-box"
           @dragover.prevent="onDragOver"
           @dragleave="onDragLeave"
           @drop.prevent="onDrop"
           :class="{ 'dragging': isDragging }">
        <img class="upload-icon" src="../views/uploadLogo.png" alt="Upload Icon" />
        <p class="upload-text">Drag film here to upload or</p>
        <input type="file" ref="fileInput" class="file-input" @change="handleFileUpload" accept="video/*" multiple />
        <button class="choose-file-btn" @click="triggerFileInput">Choose File</button>
      </div>

      <!-- Upload Status on Right -->
      <div class="upload-status-horizontal">
        <div v-if="uploads.length > 0" class="uploaded-files-label">Uploaded Files</div>

        <div 
          v-for="(upload, index) in uploads" 
          :key="index"
          class="uploading-box-horizontal"
        >
          <div class="uploading-name">
            <img class="upload-icon-small" src="../views/FileIcon.png" alt="File Icon" />
            {{ upload.videoName }}
          </div>

          <div class="progress-bar-container">
            <div class="progress-bar" :style="{ width: upload.progress + '%' }"></div>
          </div>

          <div class="delete-circle" @click.stop="removeVideo(index)">
            &#x2716;
          </div>
        </div>
      </div>
    </div>

    <!-- Done Button -->
    <div class="done-button-container">
      <button 
        class="done-btn" 
        @click="saveVideoAndRedirect" 
        :style="{ opacity: allUploadsComplete ? 1 : 0.3 }"
        :disabled="!allUploadsComplete">
        Done
      </button>
    </div>
  </div>
</template>

<script>
import TestFile from '@/services/TestFile'
export default {
  name: "UploadPage",
  data() {
    return {
      selectedTeam: this.$route.params.teamName,
      teams: JSON.parse(localStorage.getItem('teams')) || [],
      uploads: [],
      isDragging: false,
    };
  },
  computed: {
    allUploadsComplete() {
      return this.uploads.length > 0 && this.uploads.every(upload => upload.isComplete);
    }
  },
  methods: {
    triggerFileInput() {
      this.$refs.fileInput.click();
    },
    handleFileUpload(event) {
      const files = Array.from(event.target.files);
      files.forEach(file => this.startUpload(file));
    },
    startUpload(file) {
      const uploadEntry = {
        videoName: file.name,
        file,
        progress: 0,
        isComplete: false,
      };

      this.uploads.push(uploadEntry);

      this.$nextTick(() => {
        requestAnimationFrame(() => {
          this.simulateUpload(uploadEntry);
        });
      });
    },
    simulateUpload(uploadEntry) {
      const totalSize = uploadEntry.file.size;
      let uploaded = 0;
      const chunkSize = totalSize / 100;

      const uploadStep = () => {
        if (uploaded < totalSize) {
          uploaded += chunkSize;
          const newProgress = Math.min((uploaded / totalSize) * 100, 100);
          uploadEntry.progress = newProgress;

          // Trigger Vue reactivity by replacing the object
          this.uploads.splice(this.uploads.indexOf(uploadEntry), 1, { ...uploadEntry });

          setTimeout(uploadStep, 50);
        } else {
          Object.assign(uploadEntry, { progress: 100, isComplete: true });

          // Replace the updated object again to notify Vue
          this.uploads.splice(this.uploads.indexOf(uploadEntry), 1, { ...uploadEntry });
        }
      };

      uploadStep();
    },
    removeVideo(index) {
      this.uploads.splice(index, 1);
    },
    saveVideoAndRedirect() {
      const completeUploads = this.uploads.filter(u => u.isComplete);
      if (completeUploads.length === 0) {
        console.error('No completed uploads');
        return;
      }

      let uploadedVideos = JSON.parse(localStorage.getItem('uploadedVideos')) || [];

      completeUploads.forEach(upload => {
        const fileUrl = URL.createObjectURL(upload.file);
        uploadedVideos.push({
          team: this.selectedTeam,
          video: fileUrl,
          videoName: upload.videoName,
        });
        //console.log(file)
        let formData = new FormData()
        formData.append("file",upload.file)
        TestFile.test(upload.file)
      });

      localStorage.setItem('uploadedVideos', JSON.stringify(uploadedVideos));
    

      this.$router.push({
        name: 'stats',
        params: { 
          teamName: this.selectedTeam, 
          videoName: completeUploads[0].videoName
        },
      });
    },
    onDragOver() {
      this.isDragging = true;
    },
    onDragLeave() {
      this.isDragging = false;
    },
    onDrop(event) {
      this.isDragging = false;
      const files = Array.from(event.dataTransfer.files);
      files.forEach(file => this.startUpload(file));
    }
  }
};
</script>

<style scoped>
.upload-container {
  position: relative; /* Added to anchor absolute Done button */
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start; 
  height: 85vh;
  padding-top: 40px;
  padding-left: 250px;
  font-family: 'Roboto';
  color: white;
}

.team-selection {
  display: flex;
  align-items: center;
  margin-bottom: 25px;
}

.team-text {
  font-size: 18px;
  font-weight: bold;
  margin-right: 10px;
}

.team-dropdown {
  background-color: rgba(255, 255, 255, 0.05);
  color: rgb(255, 255, 255);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 30px;
  padding: 12px 30px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.team-dropdown:hover {
  background-color: #5d5959;
}

.upload-main {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  gap: 40px;
}

.upload-box {
  background-color: transparent;
  border: 1px solid white;
  border-radius: 15px;
  width: 320px;
  height: 320px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 50px;
  margin-left: 20px;
  position: relative;
  transition: border-color 0.3s ease;
}

.upload-box.dragging {
  border-color: #FF5448;
}

.upload-icon {
  width: 100px;
  height: 100px;
  margin-top: -2%;
}

.upload-text {
  margin-top: 35px;
  font-weight: bold;
  color: white;
}

.file-input {
  display: none;
}

.choose-file-btn {
  background-color: #FF5448;
  color: white;
  border: none;
  border-radius: 30px;
  padding: 12px 30px;
  font-size: 16px;
  cursor: pointer;
  margin-top: 20px;
  transition: background-color 0.3s;
}

.choose-file-btn:hover {
  background-color: #E13C3A;
}

.upload-status-horizontal {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 450px;
  padding-left: 15px;
  max-height: 320px;
  overflow-y: auto;
}

.uploaded-files-label {
  font-size: 1.5rem;
  font-weight: bold;
  color: white;
  margin-bottom: 15px;
}

.uploading-box-horizontal {
  background-color: transparent;
  padding: 10px 15px;
  width: 100%;
  text-align: left;
  border-radius: 10px;
  font-size: 1rem;
  color: white;
  cursor: default;
  position: relative;
  border: none;
}

.uploading-name {
  font-size: 1rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: flex;
  align-items: center;
}

.uploading-name img {
  width: 28px;
  height: 30px;
  margin-right: 10px;
}

.upload-status-horizontal::-webkit-scrollbar {
  width: 6px;
}

.upload-status-horizontal::-webkit-scrollbar-thumb {
  background: #ff5448;
  border-radius: 10px;
}

.upload-status-horizontal::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
}

.progress-bar-container {
  height: 6px;
  width: 100%;
  background-color: #ccc;
  border-radius: 5px;
  margin-top: 8px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  background-color: #FF5448;
  transition: width 0.9s ease-in-out;
}

.delete-circle {
  position: absolute;
  top: 10px;
  right: 15px;
  font-size: 18px;
  background: none;
  color: white;
  border: none;
  cursor: pointer;
}

.delete-circle:hover {
  color: #FF5448;
}

.done-button-container {
  position: absolute;
  bottom: 30px;
  left: 270px;
}

.done-btn {
  background-color: #FF5448;
  color: white;
  border: none;
  border-radius: 30px;
  padding: 12px 30px;
  font-size: 16px;
  cursor: pointer;
  margin-left: 2px;
  transition: background-color 0.3s;
}

.done-btn:hover {
  background-color: #E13C3A;
}

.done-btn:disabled {
  background-color: rgba(255, 255, 255, 0.05);
  color: rgb(255, 255, 255);
  border: 2px solid rgba(255, 255, 255, 0.3);
}
</style>
