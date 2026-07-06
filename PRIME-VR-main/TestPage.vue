<template>
    <div class="about">
      <h1>This is something different</h1>
    </div>
    <div>
      <select v-model="selectedValue" @change="GameSelected">
        <option disabled value="">GameID</option>
        <option v-for="option in options" :value="option.value">
          {{ option.text }}
        </option>
      </select>
    </div>
    <div>
      <select v-model="playValue" @change="PlaySelected">
        <option disabled value="">PlayID</option>
        <option v-for="option in playOptions" :value="option.text">
          {{ option.text }}
        </option>
      </select>
    </div>
    ><div>
      <input type="file" @change="fileUpload" /> 
      <button type="button" class="btn" @click="handleClick">Base class</button>
      <button type="button" class="btn" @click="enableUnity">Base class</button>
    </div>
    <VueUnity :unity="unityContext" width="800" height="600" />
  </template>
  
  <script setup>
  //accept=".mp4, .mov"
    import TestFile from '@/services/TestFile'
    import FormData from 'form-data'
    import GetUnityInfo from '@/services/unity'
    import DBInfo from '@/services/database'
    import UnityWebgl from "unity-webgl"
    import VueUnity from 'unity-webgl/vue'
    import {onMounted, ref} from 'vue'

    //const testData = `[{"x":3.5,"y":1.0,"z":-3.5},{"x":3.5,"y":1.0,"z":3.5},{"x":-3.5,"y":1.0,"z":3.5},{"x":-3.5,"y":1.0,"z":-3.5}]`
    const text = ref("")
    const options = ref([])
    const playOptions = ref([])
    const selectedValue = ref()
    const playValue = ref()

    onMounted(async () =>{ //Gets game info and displays it as options
      let res = await DBInfo.GetGames()
      console.log(res.data)
      res.data.forEach(element => {
        options.value.push({text: "Week " + element[1] + ": " + element[2] + " vs " + element[3], value: element[0]})
      });
    })


    const unityContext = new UnityWebgl({
      loaderUrl:'./Pause.loader.js',
      dataUrl:'./Pause.data',
      frameworkUrl:'./Pause.framework.js',
      codeUrl:'./Pause.wasm'
    })

    unityContext.addUnityListener('gameStart', (msg) => {
		console.log('from Unity : ', msg)
	})

    async function handleClick(){
      console.log("The button was clickied")
      //unityContext.sendMessage('TestJson', 'unpack', testData)
      if(!isNaN(text.value)){
        let result = await GetUnityInfo.GetUnityInfo(text.value)
        console.log(JSON.stringify(result))
        //unityContext.sendMessage('TestJson','unpack',JSON.stringify(result.data))
      }
    }

    async function enableUnity() {
      unityContext.sendMessage('EventSystem','ChangeWebGL')
    }
    async function fileUpload(event){
      const file = event.target.files[0];
      //console.log(file)
      let formData = new FormData()
      formData.append("file",file)
      console.log(formData.getAll("file"))
      let res = await TestFile.test(file)
      console.log(res)
    } 

    async function GameSelected(){
      let res = await DBInfo.GetPlays(selectedValue.value)
      playOptions.value = []
      res.data.forEach(element => {
        playOptions.value.push({text: element})
      });
    }

    async function PlaySelected(){
      //console.log(playValue.value)
      let result = await GetUnityInfo.GetUnityInfo(playValue.value, selectedValue.value)
      //console.log(result)
      unityContext.sendMessage('TestJson','unpack',JSON.stringify(result.data))
    }
  </script>