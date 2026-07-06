<template>
  <div class="centered-container">
    <div class="flex justify-center items-center">
      <Title>Detector</Title>
    </div>

    <div class="mt-6 w-full flex justify-center items-center">
      <Detector />
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineAsyncComponent } from "@vue/runtime-core";

const Title = defineAsyncComponent(() => import(/*webpackChunkName:"title"*/ '../components/Title.vue'));
const Detector = defineAsyncComponent(() => import(/*webpackChunkName:"detector"*/ '../components/Detector.vue'));
</script>

<style scoped>
.centered-container {
  display: flex;
  justify-content: center;
  margin-top: -80px;
  align-items: center;
  flex-direction: column;
  height: 100vh; /* Full viewport height */
  text-align: center;
}
</style>
