<template>
  <div class="flex flex-col justify-center items-centr">
    <canvas id="canvas" width="1000" height="700" style="border: 2px solid black"></canvas>
    <button class="mt-4 p-4 bg-green" type="button" @click="addRect('green', { x: 60, y: 60 })">Add</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      canvasCtx: null,
      canvasId: 0
    }
  },
  methods: {
    addRect(color = "red", Coords = { x: 0, y: 0 }) {

      this.canvasCtx.lineWidth = "25";
      this.canvasCtx.lineJoin = "bevel";
      this.canvasCtx.fillStyle = color;
      this.canvasCtx.fillRect(Coords.x, Coords.y, 60, 60);

    }
  },
  mounted() {
    const canvas = document.getElementById("canvas");

    const ctx = canvas.getContext("2d");

    this.canvasCtx = ctx

    this.addRect();


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
  margin-top: 60px;
}
</style>
