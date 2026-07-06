<template>
  <div class="signin-background">
    <!-- Bottom Right Blob Background -->
    <div class="blob-clipper">
      <div class="blob-background">
        <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
          <path
            fill="#FF5448"
            d="M30.8,-37.5C44.1,-32.6,62.1,-29.3,73,-18.4C83.8,-7.5,87.6,11.1,76.7,18.8C65.8,26.6,40.3,23.5,25.1,26.3C9.9,29.2,4.9,37.9,-5.8,45.9C-16.5,53.9,-33,61,-43,56.5C-53.1,52,-56.6,35.8,-59.5,20.8C-62.4,5.8,-64.7,-8,-57.6,-15.4C-50.6,-22.8,-34.3,-23.8,-23.1,-29.4C-12,-35,-6,-45.2,1.4,-47.1C8.7,-48.9,17.4,-42.4,30.8,-37.5Z"
            transform="translate(100 100)"
          />
        </svg>
      </div>
    </div>

    <div class="signin-main-layout">
      <!-- Left Side -->
      <div class="left-content">
        <h1 class="title">VR</h1>

        <!-- Subtitle -->
        <p class="subtitle">
          Before launching VR, make sure your headset is properly connected to your computer and powered on. For the best experience, ensure your VR software is running and your play area is clear.
        </p>
        
        <!-- Dropdowns -->
        <div class="dropdown-container">
          <select v-model="selectedValue" @change="GameSelected">
            <option disabled value="">GameID</option>
            <option v-for="option in options" :value="option.value">
              {{ option.text }}
            </option>
          </select>

          <select v-model="playValue" @change="PlaySelected">
            <option disabled value="">PlayID</option>
            <option v-for="option in playOptions" :value="option.text">
              {{ option.text }}
            </option>
          </select>
        </div>


        <!-- Launch Button -->
        <button class="login-button" @click="enterVR">LAUNCH VR</button>
      </div>

      <!-- Right Side -->
      <div class="right-visuals">
        <div id="unity-container">
         <div id="unity-canvas-container">
           <canvas id="unity-canvas" style="width: 640px; height: 420px;"></canvas>
         </div>
         <div id="unity-loading-bar">
         <div id="unity-logo"></div>
         <div id="unity-progress-bar-empty">
           <div id="unity-progress-bar-full"></div>
         </div>
        </div>
      </div>
    </div>
    
      
    </div>
  </div>
</template>

<script>
import 'webxr-polyfill';

import { onMounted, ref } from "vue";
import DBInfo from "@/services/database";
import GetUnityInfo from "@/services/unity";

export default {
  name: 'WebXRUnityPage',
  setup(){
    const options = ref([]);
    const playOptions = ref([]);
    const selectedValue = ref();
    const playValue = ref();

    onMounted(async () =>{ //Gets game info and displays it as options
      let res = await DBInfo.GetGames()
      res.data.forEach(element => {
        options.value.push({text: "Week " + element[1] + ": " + element[2] + " vs " + element[3], value: element[0]})
      });
    })

    async function GameSelected() {
        let res = await DBInfo.GetPlays(selectedValue.value);
        playOptions.value = [];
        res.data.forEach((element) => {
        playOptions.value.push({ text: element });
      });
    }

    async function PlaySelected() {
      await GetUnityInfo.GetUnityInfo(playValue.value, selectedValue.value);
    }

    return{
      options,
      playOptions,
      selectedValue,
      playValue,
      GameSelected,

    }
  },
  data() {
    return {
      vrDisabled: true,  // Disable the VR button until everything is ready
      unityInstance: null,
    };
  },
  mounted() {
    this.loadUnity();
  },
  methods: {
    // Load Unity WebGL instance
    loadUnity() {
      const buildUrl = "Build";
      const loaderUrl = buildUrl + "/Builds.loader.js";
      const config = {
        dataUrl: buildUrl + "/Builds.data",
        frameworkUrl: buildUrl + "/Builds.framework.js",
        codeUrl: buildUrl + "/Builds.wasm",
        streamingAssetsUrl: "StreamingAssets",
        companyName: "De-Panther",
        productName: "Unity WebXR Export",
        productVersion: "0.1",
      };

      const script = document.createElement("script");
      script.src = loaderUrl;
      script.onload = () => {
        createUnityInstance(document.getElementById("unity-canvas"), config, this.onUnityProgress)
          .then((instance) => {
            this.unityInstance = instance;
            console.log(this.unityInstance)
            document.getElementById("unity-loading-bar").style.display = "none";
            this.checkWebXRSupport();
          })
          .catch((message) => {
            alert(message);
          });
      };
      document.body.appendChild(script);
    },
    

    // Handle progress bar during Unity load
    onUnityProgress(progress) {
      const progressBarFull = document.getElementById("unity-progress-bar-full");
      progressBarFull.style.width = `${100 * progress}%`;
    },

    // Check if WebXR is supported and enable the VR button
    checkWebXRSupport() {
      if (this.unityInstance.Module.WebXR) {
        this.vrDisabled = false;
      }
    },

    // Start the VR session
    enterVR() {
      if (this.unityInstance && this.unityInstance.Module.WebXR) {
        this.unityInstance.Module.WebXR.toggleVR();
      }
    },

    async PlaySelected(){
      console.log(this.playValue)
      let result = await GetUnityInfo.GetUnityInfo(this.playValue, this.selectedValue)
      //console.log(result)
      this.unityInstance.SendMessage('TestJson','unpack',JSON.stringify(result.data))
    }
  },
};
</script>

<style scoped>
html, body {
  background: linear-gradient(185deg, rgb(34, 32, 32) 90%, red 100%);
  height: 100%;
  margin: 0;
  padding: 0;
  height: 100vh;
}

.signin-background {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  position: relative;
  overflow: hidden;
}

/* Bottom Right Blob */
.blob-clipper {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 600px;
  height: 600px;
  z-index: -1;
}

.blob-background {
  position: absolute;
  bottom: -800px;
  right: -600px;
  width: 1600px;
  height: 1600px;
  pointer-events: none;
}

.signin-main-layout {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: flex-start;
  width: 100%;
  max-width: 1300px;
  padding: 80px 40px 40px;
  box-sizing: border-box;
}

.left-content {
  display: flex;
  flex-direction: column;
  width: 55%;
  align-items: flex-start;
}

.right-visuals {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 45%;
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
}

.title {
  font-size: 3rem;
  font-weight: 700;
  color: white;
  margin-top: 0;
  margin-bottom: 20px;
}

.dropdown-container {
  display: flex;
  flex-direction: row;
  margin-top: 10px;
  gap: 10px;
}

.dropdown-container select {
  background-color: #FF5448;
  cursor: pointer;
  transition: background-color 0.3s;
  padding: 12px 30px;
  font-size: 16px;
  border: none;
  border-radius: 30px;
  color: white;
}

.dropdown-container select option {
  color: white;
  background-color: #FF5448;
}

.dropdown-container select option:disabled {
  color: white;
  background-color: #FF5448;
}

.subtitle {
  font-size: 1.25rem;
  font-weight: 400;
  color: white;
  max-width: 100%;
  text-align: left;
  margin-top: 25px;
}

.login-button {
  width: 100%;
  max-width: 350px;
  height: 70px;
  background-color: #FF5448;
  color: white;
  font-size: 1.6rem;
  font-weight: 700;
  border: none;
  border-radius: 60px;
  margin-top: 40px;
  cursor: pointer;
  align-self: flex-start;
}

.login-button:hover {
  background-color: #e14b39;
}

</style>
