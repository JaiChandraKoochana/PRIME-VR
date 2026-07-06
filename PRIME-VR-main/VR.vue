<template>
  <div class="signin-background">
    <div class="sidebar">
      <h1 class="title">GAMES</h1>
      <div class="video-container">
        <div 
          v-for="(video, index) in videos" 
          :key="index"
          class="video-namebox"
          @click="playVideo(video)">
          
          <div class="video-name">
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
          
          <!-- Edit button -->
          <div class="edit-circle" @click.stop="startEdit(index, video.videoName)">
            ✎ <!-- Edit symbol -->
          </div>
          
          <!-- Delete button -->
          <div class="delete-circle" @click.stop="deleteVideo(index)">
            &#x2716; <!-- Cross symbol (x) -->
          </div>
        </div>
      </div>
    </div>

    <div class="video-container-main">
      <h1 class="title2">{{ teamName }}</h1>
      <div class="video-box-container">
        <div class="video-box" id="DADADA">
          <div class="video-inner-box">
            <video v-if="selectedVideo" controls :src="selectedVideo" class="full-video" @loadeddata="onVideoLoaded" @error="onVideoError"/>
          </div>
        </div>
      </div>
      <div v-if="videoName" class="video-name-display">
        <h2>Video: {{ videoName }}</h2>
      </div>
      <div class="buttons-container">
        <button class="launch-vr-button" @click="launchInVR">LAUNCH IN VR</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['teamName'],  
  data() {
    return {
      videos: [],
      selectedVideo: null,  
      videoName: this.$route.params.videoName || '',  
      editingVideoIndex: null,
      editedVideoName: '',
    };
  },
  methods: {
    launchInVR() {
      console.log('Launch in VR clicked');
    },
    playVideo(video) {
      this.selectedVideo = video.video;
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
      }
    },
    highlightText() {
      this.$nextTick(() => {
        const input = document.querySelector('.edit-input');
        input.select();
      });
    }
  },
  created() {
    const uploadedVideos = JSON.parse(localStorage.getItem('uploadedVideos')) || [];
    this.videos = uploadedVideos.filter(video => video.team === this.teamName);
  }
};
</script>

<style scoped>
html, body {
  background: linear-gradient(185deg, rgb(34, 32, 32) 80%, red 100%);
  height: 100%;
  margin: 0;
  padding: 0;
  height: 100vh;
}

.signin-background {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  position: relative;
  width: 100%;
}

.video-container-main {
  font-family: 'Roboto';
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
  max-width: 750px;
  text-align: left;
  padding: 20px;
  box-sizing: border-box;
  height: 100%;
  margin-top: 2%;
}

.title {
  font-size: 2rem;
  font-weight: 700;
  color: white;
  margin-top: 0;
  margin-bottom: 20px;
}

.title2 {
  font-size: 2rem;
  font-weight: 700;
  color: white;
  margin-top: 0;
  margin-left: 60px;
}

.sidebar {
  position: fixed;
  top: 140px;
  right: 0;
  width: 420px;
  height: 520px;
  background-color: rgba(93, 89, 89, 0.5);
  color: #fff;
  padding-top: 20px;
  padding-left: 15px;
  padding-right: 20px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 100;
  border: 3px solid #9D9D9D;
  border-radius: 15px;
  overflow: visible; /* Ensure overflow is visible in the parent container */
}

.video-container {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-top: 10px;
  width: 100%;
  max-height: 400px;
  overflow-y: auto;
  overflow: visible;
  position: relative; /* Ensure it's positioned properly for z-index stacking */
}

.video-container::-webkit-scrollbar {
  width: 8px;
  border-radius: 10px;
}

.video-container::-webkit-scrollbar-track {
  background-color: #FF5448;
  border-radius: 10px;
}

.video-container::-webkit-scrollbar-thumb {
  background-color: #FF9991;
  border-radius: 10px;
  border: 2px solid #FF5448;
  min-height: 30px;
}

.video-container::-webkit-scrollbar-thumb:hover {
  background-color: #FF6660;
}

.video-namebox {
  background-color: #FF5448;
  padding: 14px;
  width: 300px;
  text-align: center;
  border-radius: 20px;
  font-size: 1.1rem;
  color: white;
  margin: 0 auto;
  cursor: pointer;
  transition: transform 0.2s, background-color 0.2s;
  position: relative;
  z-index: 1; /* Ensure the card is above other elements */
  overflow: visible; /* Allow overflow of the rectangle */
}

.video-namebox:hover::after {
  content: "";
  position: absolute;
  bottom: -120px; /* Increased from -100px */
  left: 0;
  width: 100%;
  height: 120px; /* Height of the rectangle */
  background-color: #FFBDB8; /* Updated color */
  border-radius: 20px; /* Rounded corners */
  animation: slideDown 0.5s ease-in-out;
  z-index: 0; /* Ensure the rectangle is below the card */
  overflow: visible; /* Allow the rectangle to overflow from the card */
  visibility: hidden;
  z-index: 20;
  padding-top: 20px;
  padding-left: 15px;
  padding-right: 20px;
  padding-bottom: 20px; /* Add more padding to prevent cut-off */
}



/* Add the hover effect for the rectangle */
.video-namebox:hover::after {
  content: "";
  position: absolute;
  bottom: -170px; /* Position it just below the card */
  left: 0;
  width: 100%;
  height: 180px; /* Increased height for the longer rectangle */
  background-color: #FFBDB8; /* Updated color */
  border-radius: 20px; /* Rounded corners */
  animation: slideDown 0.5s ease-in-out;
  visibility: visible;
}

/* Slide down animation */
@keyframes slideDown {
  0% {
    transform: translateY(-20px);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

.delete-circle {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 24px;
  height: 24px;
  background-color: rgba(128, 128, 128, 0.5);
  border-radius: 50%;
  display: none;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  color: white;
  font-weight: bold;
}

.video-namebox:hover .delete-circle {
  display: flex; /* Show delete button on hover */
}

.video-namebox:hover .edit-circle {
  display: flex;
}

.edit-circle {
  position: absolute;
  top: 10px;
  right: 40px;
  width: 24px;
  height: 24px;
  background-color: rgba(128, 128, 128, 0.5);
  border-radius: 50%;
  display: none;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 12px;
  color: white;
  font-weight: bold;
}

.edit-input {
  width: 100%;
  padding: 5px;
  font-size: 1rem;
  background-color: transparent;
  border: none;
  color: white;
  outline: none;
}

.video-box-container {
  margin-top: 20px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 79px;
}

.video-box {
  background-color: #FF5448;
  padding: 15px;
  width: 100%;
  max-width: 700px;
  height: 350px;
  text-align: center;
  border-radius: 20px;
  font-size: 1.1rem;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.video-inner-box {
  background-color: #DADADA;
  width: 100%;
  height: 100%;
  border-radius: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.full-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.buttons-container {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-top: 30px;
  padding: 0 20px;
  margin-left: 80px;
}

.launch-vr-button {
  background-color: #FF5448;
  color: white;
  font-size: 1.2rem;
  padding: 12px 30px;
  border-radius: 30px;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.launch-vr-button:hover {
  background-color: #e4150b;
}
</style>
