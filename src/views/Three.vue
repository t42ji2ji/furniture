<template lang="pug">
  .three(ref="three")


</template>

<script>
// @ is an alias to /src
import * as THREE from "three";
export default {
  name: "Home",
  components: {},
  mounted() {
    this.init();
    this.animate();
  },
  data() {
    return {
      camera: null,
      scene: null,
      geometry: null,
      material: null,
      mesh: null,
      renderer: null,
    };
  },
  methods: {
    init() {
      this.camera = new THREE.PerspectiveCamera(
        70,
        window.innerWidth / window.innerHeight,
        0.01,
        10
      );
      this.camera.position.z = 1;

      this.scene = new THREE.Scene();

      this.geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);
      this.material = new THREE.MeshNormalMaterial();

      this.mesh = new THREE.Mesh(this.geometry, this.material);
      this.scene.add(this.mesh);

      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight * 0.9);
      this.$refs.three.appendChild(this.renderer.domElement);
    },

    animate() {
      requestAnimationFrame(this.animate);

      this.mesh.rotation.x += 0.01;
      this.mesh.rotation.y += 0.02;

      this.renderer.render(this.scene, this.camera);
    },
  },
};
</script>

<style lang="scss" scoped>
.three {
  height: 90vh;
}
</style>
