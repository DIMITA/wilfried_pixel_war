<template>
  <div class="flex flex-col w-full justify-center items-center">
    <div style="width: 1000px; height: 700px;">
      <canvas id="canvas" style="border: 2px solid black"></canvas>
    </div>

    <div class="flex flex-wrap -mx-3 w-1/3 my-6">
      <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
        <label class="block tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
          Coordonée X
        </label>
        <input v-model="x"
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-red-500 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
          id="grid-first-name" type="number" min="0" max="19" placeholder="Coordonée X entre 0 et 19">

      </div>
      <div class="w-full md:w-1/2 px-3">
        <label class="block tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-last-name">
          Coordonée Y
        </label>
        <input v-model="y"
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-red-500 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
          id="grid-first-name" type="number" min="0" max="13" placeholder="Coordonée Y entre 0 et 13">
      </div>
    </div>
    <LvColorpicker label="Choix de la couleur" v-model="color" :clearable="true" :bottomBar="true" :colors="[
      '#F44336',
      '#E91E63',
      '#9C27B0',
      '#673AB7',
      '#3F51B5',
      '#2196F3',
      '#03A9F4',
      '#00BCD4',
      '#009688',
      '#4CAF50',
      '#8BC34A',
      '#CDDC39',
      '#FFEB3B',
      '#FFC107',
      '#FF9800',
      '#795548'
    ]" />
    <br>
    <button @click="addRect()"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 border border-blue-700 rounded my-4">
      Enrégistrer
    </button>


  </div>
</template>

<script>
import LvColorpicker from 'lightvue/color-picker';

export default {
  name: 'App',
  components: {
    LvColorpicker
  },
  data() {
    return {
      canvasCtx: null,
      canvasId: 0,
      color: "black",
      x: 0,
      y: 0
    }
  },
  watch: {
    color(newVal) {
      console.log(newVal);
    }
  },
  methods: {
    updateColor(eventData) {
      this.color = eventData.cssColor
    },
    addRect() {

      if (this.x > 13 || this.x < 0) {
        alert("Désolé les cordonnées X doivent être compris entre 0 et 19 comme indiqué")
        return
      }

      if (this.y > 19 || this.y < 0) {
        alert("Désolé les cordonnées Y doivent être compris entre 0 et 13 comme indiqué")
        return
      }
      this.canvasCtx.lineWidth = "25";
      this.canvasCtx.lineJoin = "bevel";
      this.canvasCtx.fillStyle = this.color;
      this.canvasCtx.fillRect(this.x * 50, this.y * 50, 50, 50);

    }
  },
  mounted() {
    const canvas = document.getElementById("canvas");
    canvas.width = 1000;
    canvas.height = 700;

    const ctx = canvas.getContext("2d");

    this.canvasCtx = ctx


  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 50px;
}
</style>
