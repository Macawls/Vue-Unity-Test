<script setup>
import UnityWebgl from "unity-webgl";
import VueUnity from "unity-webgl/vue";
import { reactive } from "vue";

const UnityContextState = reactive({
  loadProgress: 0,
  loadComplete: false,
});

const UnityContext = new UnityWebgl({
  loaderUrl: "WebGL/Build/WebGL.loader.js",
  dataUrl: "WebGL/Build/WebGL.data",
  frameworkUrl: "WebGL/Build/WebGL.framework.js",
  codeUrl: "WebGL/Build/WebGL.wasm",
});

UnityContext.on("progress", (val) => {
  UnityContextState.loadProgress = val;
});

UnityContext.on("mounted", () => {
  UnityContextState.loadComplete = true;
});
</script>

<template>
  <div style="width: 100%; height: 100%; display: flex; align-items: center">
    <div>
      <VueUnity
        :unity="UnityContext"
        class="unity-container"
        :class="{
          'animate__animated animate__fadeIn': UnityContextState.loadComplete,
        }"
      />
      <p v-if="!UnityContextState.loadComplete" style="font-size: x-large">
        Loading: {{ (UnityContextState.loadProgress * 100).toFixed(1) }}%
      </p>
    </div>
    <div
      v-if="UnityContextState.loadComplete"
      style="
        position: absolute;
        display: flex;
        flex-direction: column;
        gap: 30px;
        justify-content: space-evenly;
        z-index: 1;
        padding-left: 30px;
      "
    >
      <button @click="UnityContext.send('Cameras', 'SetActive', 0)">
        Back
      </button>
      <button @click="UnityContext.send('Cameras', 'SetActive', 1)">
        Side
      </button>
      <button @click="UnityContext.send('Cameras', 'SetActive', 2)">
        Front
      </button>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");

.unity-container {
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.07) 0px 1px 2px, rgba(0, 0, 0, 0.07) 0px 2px 4px,
    rgba(0, 0, 0, 0.07) 0px 4px 8px, rgba(0, 0, 0, 0.07) 0px 8px 16px,
    rgba(0, 0, 0, 0.07) 0px 16px 32px, rgba(0, 0, 0, 0.07) 0px 32px 64px;
}
</style>
