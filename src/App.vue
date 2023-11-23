<template>
  <div class="flex flex-col w-full justify-center items-center">
    <div style="width: 1000px; height: 700px;" class="mb-14">
      <canvas width="1000" height="700" id="canvas" style="border: 2px solid black"></canvas>
    </div>

    <button @click="downloadImg()"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 border border-blue-700 rounded my-4">
      Télécharger l'image
    </button>

    <div class="flex flex-wrap -mx-3 w-1/2 my-6">
      <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
        <label class="block tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
          Coordonée X
        </label>
        <input v-model="x"
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-black rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
          id="grid-x" type="number" min="0" max="19" placeholder="Coordonée X entre 0 et 19">
      </div>
      <div class="w-full md:w-1/2 px-3">
        <label class="block tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-last-name">
          Coordonée Y
        </label>
        <input v-model="y"
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-black rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
          id="grid-y" type="number" min="0" max="13" placeholder="Coordonée Y entre 0 et 13">
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

    <button @click="addPixel()"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 border border-blue-700 rounded my-4">
      Enrégistrer le pixel
    </button>

  </div>
</template>

<script>
import LvColorpicker from 'lightvue/color-picker';
import { io } from 'socket.io-client'
// import { ref } from 'vue'

let socket = io(process.env.VUE_APP_BASE_WEB_SOCKET_URL);

export default {
  name: 'App',
  components: {
    LvColorpicker
  },
  data() {
    return {
      pixels: [],
      canvasCtx: null,
      canvasId: 0,
      color: "black",
      x: null,
      y: null
    }
  },
  methods: {
    updateColor(eventData) {
      this.color = eventData.cssColor
    },

    addPixel() {
      if (!this.x || this.x > 19 || this.x < 0) {
        alert("Désolé les cordonnées X doivent être compris entre 0 et 19 comme indiqué")
        return
      }

      if (!this.y || this.y > 13 || this.y < 0) {
        alert("Désolé les cordonnées Y doivent être compris entre 0 et 13 comme indiqué")
        return
      }

      socket.emit("createPixel", {
        color: this.color,
        coordx: this.x,
        coordy: this.y,
      }, (res) => {
        console.log(res);
      })
    },

    downloadImg() {

      let imagesByTag = document.getElementsByTagName('img')
      Array.prototype.forEach.call(imagesByTag, (element, key) => {
        var link = document.createElement("a");
        const src = element.src
        if (!src.includes('data')) return;
        link.href = src;
        const exten = (src.match(/[^:]\w+\/[\w-+\d.]+(?=;|,)/)[0]).substring(6)
        link.download = `images-ia-${key}.${exten}`
        console.log(exten);
        link.click();
      });

      const canvas = document.getElementById("canvas");
      var link = document.createElement("a");
      link.download = "image.png";
      canvas.toBlob(function (blob) {
        link.href = URL.createObjectURL(blob);
        console.log(blob);
        console.log(link.href);
        link.click();
      }, 'image/png');
    }
  },
  mounted() {
    const canvas = document.getElementById("canvas");

    const ctx = canvas.getContext("2d");

    this.canvasCtx = ctx;

    canvas.width = 1000;
    canvas.height = 700;

    socket.emit("findAllPixel", {}, (res) => {
      console.log(res);
      this.pixels = res
      res.forEach(element => {
        this.canvasCtx.lineWidth = "25";
        this.canvasCtx.lineJoin = "bevel";
        this.canvasCtx.fillStyle = element.color;
        this.canvasCtx.fillRect(element.coordx * 50, element.coordy * 50, 50, 50);
      });
    })

    socket.on('message', (res) => {
      if (!res) return;
      this.canvasCtx.lineWidth = "25";
      this.canvasCtx.lineJoin = "bevel";
      this.canvasCtx.fillStyle = res.color;
      this.canvasCtx.fillRect(res.coordx * 50, res.coordy * 50, 50, 50);
    })

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
