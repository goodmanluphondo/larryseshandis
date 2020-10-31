<template>
  <div id="app">
    <div id="canvas"></div>
    <router-view />
  </div>
</template>
<script>
import {Curtains, Plane} from "curtainsjs";

export default {
  mounted: function() {
    this.runCurtainsJS()
  },
  methods: {
    runCurtainsJS() {
      // set up our WebGL context and append the canvas to our wrapper
      const curtains = new Curtains({
          container: "canvas"
      });
      
      // get our plane element
      const planeElement = document.getElementsByClassName("plane")[0];
      
      // set our initial parameters (basic uniforms)
      const params = {
          vertexShaderID: "plane-vs", // our vertex shader ID
          fragmentShaderID: "plane-fs", // our fragment shader ID
          uniforms: {
              time: {
                  name: "uTime", // uniform name that will be passed to our shaders
                  type: "1f", // this means our uniform is a float
                  value: 0,
              },
          },
      };

      // create our plane using our curtains object, the bound HTML element and the parameters
      const plane = new Plane(curtains, planeElement, params);
      
      plane.onRender(() => {
          // use the onRender method of our plane fired at each requestAnimationFrame call
          plane.uniforms.time.value++; // update our time uniform value
      });
    }
  }
}
</script>
<style>
body {
    position: relative;
    width: 100%;
    height: 100vh;
    margin: 0;
    overflow: hidden;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#canvas {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
}
.plane {
    width: 80%;
    height: 80vh;
    margin: 10vh auto;
}
.plane img {
    width: 70%;
    display: none;
}
</style>
