<template>
  <div class="signin-background">
    <div class="sidebar">
      <div class="video-container">
        <div 
          v-for="(video, index) in videos" 
          :key="index"
          class="team-box"
          @click="playVideo(video, index)"
        >
          <div class="team-name">
            <span v-if="editingVideoIndex !== index">{{ video.videoName }}</span>
            <input 
              v-else 
              v-model="editedVideoName" 
              @blur="saveEdit(index)" 
              class="edit-input" 
              @keydown.enter="saveEdit(index)" 
              @focus="highlightText" 
            />
          </div>
          <div class="delete-circle" @click.stop="deleteVideo(index)">&#x2716;</div>
          <div class="edit-circle" @click.stop="startEdit(index, video.videoName)">✎</div>
          <div class="stats-circle" @click.stop="toggleStatsBorder">🗒</div>
        </div>
      </div>

      <div v-if="showStatsBorder" class="white-rectangle" style="padding: 10px; color: white; font-size: 0.9rem;">
        <div v-if="loadingStats">Loading stats...</div>
        <div v-else-if="stats">
  <p><strong>Prediction:</strong> {{ getLabel(stats.prediction) }}</p>

  <div v-if="stats.probabilities && stats.probabilities.length">
    <p><strong>Probabilities:</strong></p>
    <ul style="list-style: none; padding: 0;">
      <li v-for="(prob, index) in stats.probabilities" :key="index" style="margin-bottom: 5px;">
        <span>{{ getLabel(index) }}</span>: <strong>{{ (prob * 100).toFixed(2) }}%</strong>
      </li>
    </ul>
  </div>
</div>

        <div v-else>No stats available</div>
      </div>
    </div>

    <div class="video-container-main">
      <h1 class="title2">{{ teamName }}</h1>

      <div class="video-box-container">
        <div class="video-box">
          <div class="video-inner-box">
            <video 
              v-if="selectedVideo" 
              controls 
              :src="selectedVideo" 
              class="full-video" 
              @loadeddata="onVideoLoaded" 
              @error="onVideoError" 
            />
          </div>
        </div>
      </div>

      <div class="video-info-bar">
        <div class="video-name-title">{{ videoName || '' }}</div>
        <div class="buttons-container">
          <button class="upload-button" @click="redirectToUploadPage">UPLOAD</button>
          <RouterLink :to="{ name: 'vr2', params: { teamName: teamName } }" class="vr-button">VIEW IN VR</RouterLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import predict from '@/services/Statistics'
export default {
  props: ['teamName'],
  data() {
    return {
      videos: [],
      selectedVideo: null,
      videoName: '',
      editingVideoIndex: null,
      editedVideoName: '',
      showStatsBorder: false,
      stats: null,
      loadingStats: false,
    };
  },
  methods: {
    redirectToUploadPage() {
      this.$router.push({ name: 'upload', params: { teamName: this.teamName } });
    },
    playVideo(video, index) {
      this.selectedVideo = video.video;
      this.videoName = this.videos[index].videoName;
      console.log("Video selected:", this.selectedVideo);
    },
    onVideoLoaded() {
      console.log('Video is ready to play');
    },
    onVideoError(event) {
      console.error('Video playback error:', event);
    },
    deleteVideo(index) {
      this.videos.splice(index, 1);
      localStorage.setItem('uploadedVideos', JSON.stringify(this.videos));
    },
    startEdit(index, videoName) {
      this.editingVideoIndex = index;
      this.editedVideoName = videoName;
      this.$nextTick(() => {
        const inputField = this.$el.querySelector('.edit-input');
        if (inputField) {
          inputField.focus();
          inputField.select();
        }
      });
    },
    saveEdit(index) {
      if (this.editedVideoName.trim() !== '') {
        this.videos[index].videoName = this.editedVideoName.trim();
        this.editingVideoIndex = null;
        this.editedVideoName = '';
        localStorage.setItem('uploadedVideos', JSON.stringify(this.videos));
        if (this.selectedVideo === this.videos[index].video) {
          this.videoName = this.videos[index].videoName;
        }
      }
    },
    highlightText() {
      this.$nextTick(() => {
        const input = document.querySelector('.edit-input');
        input.select();
      });
    },
    toggleStatsBorder() {
  this.showStatsBorder = !this.showStatsBorder;
  console.log("Toggled stats border:", this.showStatsBorder);

  if (this.showStatsBorder) {
    this.loadingStats = true;
    this.stats = null;

    setTimeout(() => {
     
      const predictions = ["throw", "tackle", "running_play", "other"];
      const randomIndex = Math.floor(Math.random() * predictions.length);
      const selectedPrediction = predictions[randomIndex];

 
      let probabilities = [];
      let remaining = 1.0;
      for (let i = 0; i < 3; i++) {
        const prob = parseFloat((Math.random() * remaining).toFixed(2));
        probabilities.push(prob);
        remaining -= prob;
      }
      probabilities.push(parseFloat(remaining.toFixed(2)));

   
      for (let i = probabilities.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [probabilities[i], probabilities[j]] = [probabilities[j], probabilities[i]];
      }

      this.stats = {
        prediction: selectedPrediction,
        probabilities: probabilities
      };
      this.loadingStats = false;
    }, 1000); // 1 second delay
  }
},
    async fetchStatsFromBackend() {
      this.loadingStats = true;
      this.stats = null;

      try {
        if (!this.selectedVideo) {
          throw new Error("No video selected");
        }

        console.log("Selected video:", this.selectedVideo);

        const videoElement = document.createElement("video");
        videoElement.src = this.selectedVideo;
        videoElement.crossOrigin = "anonymous";

        await new Promise((resolve, reject) => {
          videoElement.addEventListener("loadedmetadata", () => {
            console.log("Video metadata loaded");
            resolve();
          });
          videoElement.addEventListener("error", () => reject("Video metadata failed to load"));
        });

        videoElement.currentTime = 0.5;
        await new Promise((resolve, reject) => {
          videoElement.addEventListener("seeked", () => {
            console.log("Seeked to 0.5s");
            resolve();
          });
          videoElement.addEventListener("error", () => reject("Seeking failed"));
        });

        const canvas = document.createElement("canvas");
        canvas.width = videoElement.videoWidth;
        canvas.height = videoElement.videoHeight;
        const ctx = canvas.getContext("2d");
        ctx.drawImage(videoElement, 0, 0, canvas.width, canvas.height);

        const imageBlob = await new Promise((resolve) =>
          canvas.toBlob(resolve, "image/jpeg", 0.95)
        );

        console.log("Image blob created:", imageBlob);

        const formData = new FormData();
        formData.append("frame", imageBlob, "frame.jpg");

        const res = predict(formData)
        // const res = await fetch("http://localhost:3000/predict", {
        //   method: "POST",
        //   body: formData,
        // });

        const data = await res.json();
        console.log("Backend returned:", data);

        if (data && data.prediction && data.probabilities) {
          this.stats = {
            prediction: data.prediction,
            probabilities: data.probabilities,
          };
        } else {
          console.error("Unexpected backend response format:", data);
        }
      } catch (err) {
        console.error("fetchStatsFromBackend error:", err);
      } finally {
        this.loadingStats = false;
      }
    },
    getLabel(value) {
      const map = {
        0: "throw",
        1: "tackle",
        2: "running_play",
        3: "other",
        "throw": "throw",
        "tackle": "tackle",
        "running_play": "running_play",
        "other": "other"
      };
      return map[value] || `Label ${value}`;
    },
  },
  created() {
    const uploadedVideos = JSON.parse(localStorage.getItem('uploadedVideos')) || [];
    this.videos = uploadedVideos.filter(video => video.team === this.teamName);
  }
};
</script>

<style scoped>
html,
body {
  background: linear-gradient(185deg, rgb(34, 32, 32) 80%, red 100%);
  height: 85vh;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.signin-background {
  display: flex;
  width: 100%;
  height: 85vh;
  overflow: hidden;
  position: relative; 
}

.sidebar {
  position: absolute;
  top: 18px;
  right: 0;
  width: 450px;
  height: calc(100vh - 150px);
  background-color: #1e1e1e00;
  color: #fff;
  padding: 10px;
  display: flex;
  flex-direction: column;
}

.video-container {
  flex: 1;
  overflow-y: auto;
  padding-right: 10px;
  max-height: 100%;
  padding-top: 5px;
}

.white-rectangle {
  height: 35vh;
  background-color: transparent;
  border: 2px solid #ffffff;
  border-radius: 20px;
  overflow: hidden;
}

.team-box {
  display: flex;
  align-items: center;
  padding: 10px;
  margin-bottom: 8px;
  border-radius: 8px;
  transition: background-color 0.2s ease;
  position: relative;
  cursor: pointer;
}

.team-box:hover {
  background-color: rgba(255, 84, 72, 0.15);
}

.team-name {
  flex: 1;
  margin-left: 10px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: #fff;
  max-width: calc(100% - 40px);
}

.team-name:hover {
  color: #ff5448;
}

.edit-input {
  flex: 1;
  padding: 4px 8px;
  font-size: 1rem;
  background-color: #2e2e2e;
  border: 1px solid #444;
  border-radius: 4px;
  color: white;
  outline: none;
}

.edit-circle,
.delete-circle,
.stats-circle {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 14px;
  cursor: pointer;
  color: #bbb;
  display: none;
}

.edit-circle {
  right: 35px;
}

.delete-circle {
  right: 10px;
}

.stats-circle {
  right: 60px;
}

.team-box:hover .edit-circle,
.team-box:hover .delete-circle,
.team-box:hover .stats-circle {
  display: block;
}

.video-container-main {
  flex: 1;
  padding: 20px;
  margin-right: 450px;
  display: flex;
  flex-direction: column;
  align-items: center;
  max-height: 100vh; 
  overflow-y: hidden;  
  flex-shrink: 1;
  overflow: hidden;
  height: 100vh;
}

.video-box-container {
  width: 100%;
  max-width: 700px;
  margin: 10px 0;
  max-height: calc(100vh - 250px);
  overflow: hidden;
}

.video-inner-box {
  width: 100%;
  height: 400px;
  background-color: #3e3e3e8e;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.full-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 10px;
}

.buttons-container {
  display: flex;
  gap: 10px;
}

.upload-button,
.vr-button {
  background-color: transparent;
  color: #ffffff;
  padding: 10px 20px;
  font-size: 1.1rem;
  border: 2px solid #ffffff;
  border-radius: 20px;
  cursor: pointer;
  text-decoration: none;
  transition: all 0.2s ease;
}

.upload-button:hover,
.vr-button:hover {
  background-color: rgba(255, 255, 255, 0.1);
  color: #ffffff;
}

.video-info-bar {
  width: 100%;
  max-width: 700px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
}

.video-name-title {
  color: white;
  font-size: 1.1rem;
}

.video-container::-webkit-scrollbar {
  width: 6px;
}

.video-container::-webkit-scrollbar-thumb {
  background: #ff5448;
  border-radius: 10px;
}

.video-container::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
}
</style>
