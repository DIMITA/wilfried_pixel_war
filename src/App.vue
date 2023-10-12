<template>
  <div class="flex justify-center w-full items-centr">
    <canvas id="canvas" style="border: 2px solid black"></canvas>
    <button type="button" @click="addRect('green', { x: 60, y: 60 })">Add</button>
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
    }
  },
  methods: {
    addRect(color = "darkorange", canvas = { x: 0, y: 0 }) {

      this.canvasCtx.rect(canvas.x, canvas.y, 60, 60);
      this.canvasCtx.fillStyle = color;
      this.canvasCtx.fill();

    }
  },
  mounted() {
    const canvas = document.getElementById("canvas");
    canvas.width = 1000;
    canvas.height = 700;
    console.log(canvas);
    const ctx = canvas.getContext("2d");

    this.canvasCtx = ctx

    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, 1000, 700);

    this.addRect()

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
