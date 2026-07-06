<template>
  <div>
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
    <div id="unity-container">
      <div id="unity-canvas-container">
        <canvas id="unity-canvas" style="width: 100%; height: 100%;"></canvas>
      </div>
      <div id="unity-loading-bar">
        <div id="unity-logo"></div>
        <div id="unity-progress-bar-empty">
          <div id="unity-progress-bar-full"></div>
        </div>
      </div>
      <div id="unity-footer">
        <div id="unity-webgl-logo"></div>
        <button id="entervr" value="Enter VR" @click="enterVR" :disabled="vrDisabled">Enter VR</button>
        <div id="unity-webxr-link">Using <a href="https://github.com/De-Panther/unity-webxr-export" target="_blank" title="WebXR Export">WebXR Export</a></div>
        <div id="unity-build-title">Unity WebXR Export</div>
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
      const loaderUrl = buildUrl + "/Reverse.loader.js";
      const config = {
        dataUrl: buildUrl + "/Reverse.data",
        frameworkUrl: buildUrl + "/Reverse.framework.js",
        codeUrl: buildUrl + "/Reverse.wasm",
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
#unity-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#unity-canvas-container {
  width: 700px;
  height: 400px;
}

#unity-loading-bar {
  display: block;
}

button {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

button:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}
</style>
