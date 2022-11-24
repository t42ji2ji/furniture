<template lang="pug">
.demoPage
  .outer(style="width: 20vw; position:relative; overflow: hidden;")
    .drawer
      .grid
        .textImg(v-for="style in styles")
          .img(@click="selectStyle(style)" :class="{ active: choosenStyle(style)}" :style="{backgroundImage: 'url('+ require(`../assets/demo/style/${style}.jpg`) +')'}"  )
      .grid.gridRight
        .back(@click="moveTotexture(-1)" style="display:flex; justify-content: center; align-items: center; cursor: pointer;") 
          .backText 返回
        .textImg(v-for="texture in textures")
          .img(@click="selectTexture(texture)" :class="{ active: choosenTexture(texture)}" :style="{backgroundImage: 'url('+ require(`../assets/demo/texture/${texture}.jpg`) +')'}"  )
      //- .activeBorder(:class="{ active: choosenTextureStyle(texture)}")
  //- .bigImg(v-if="selectedTexture && selectedStyle" :style="{backgroundImage: 'url('+ require(`../assets/demo/render/${this.selectedStyle}/臥室-${this.selectedStyle}_${this.selectedTexture}.jpg`) +')'}"  )
  a-scene.sceneStyle(device-orientation-permission-ui="enabled: false" vr-mode-ui="enabled: false" renderer="antialias: true" embedded=true v-if="selectedTexture && selectedStyle")
    a-sky(:src="require(`../assets/demo/render/${this.selectedStyle}/臥室-${this.selectedStyle}_${this.selectedTexture}.jpg`)" rotation="0 -90 0")
</template>

<script>
import * as THREE from 'aframe';

export default {
  name: 'Demo',
  data() {
    return {
      selectedTexture: '',
      selectedStyle: '',
      styles: [
        'F-1',
        'F-2',
        'F-4',
        'F',
        'H-1',
        'H-2',
        'H',
        'M7',
        'M8',
        'U1',
        'U2',
        'U3',
        'U4',
        'U5',
        'W',
        'X6-1',
        'X6',
      ],
      textures: [
        'K01',
        'K02',
        'K03',
        'K04',
        'K05',
        'K06',
        'K07',
        'K08',
        'K09',
        'U10',
        'U20',
        'U30',
        'U40',
        'U50',
        'U60',
        'U70',
        'U80',
        'Y15',
        'Y35',
        'Y45',
        'Y55',
        'Y65',
        'Y95',
        'Y105',
        'Y115',
      ],
    };
  },
  computed: {
    renderUrl() {
      return `../assets/demo/render/${this.selectedStyle}/臥室-${this.selectedStyle}_${this.selectedTexture}.jpg`;
    },
  },
  methods: {
    selectTexture(texture) {
      this.selectedTexture = texture;
      this.init();
    },
    selectStyle(style) {
      this.selectedStyle = style;
      this.moveTotexture();
      this.init();
    },
    choosenTexture(texture) {
      if (this.selectedTexture === texture) {
        return true;
      } else {
        return false;
      }
    },
    choosenStyle(style) {
      if (this.selectedStyle === style) {
        return true;
      } else {
        return false;
      }
    },
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

    // animate() {
    //   requestAnimationFrame(this.animate);

    //   this.mesh.rotation.x += 0.01;
    //   this.mesh.rotation.y += 0.02;

    //   this.renderer.render(this.scene, this.camera);
    // },
    moveTotexture(dis) {
      var tg = document.querySelector('.drawer');
      if (dis == -1) {
        tg.style.WebkitTransform = `translateX(0px)`;
      } else {
        tg.style.WebkitTransform = `translateX(-50%)`;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
canvas {
  border-radius: 10px;
}
.sceneStyle {
  margin: 10px;
  box-sizing: border-box;
  border-radius: 10px;
  max-width: 90%;
}
.demoPage {
  height: 90vh;
  position: relative;
  padding: 0vw 1vw 2vw 1vw;
  box-sizing: border-box;
  display: flex;
}

.textImg {
  width: 100%;
  aspect-ratio: 1/1;
  position: relative;
  cursor: pointer;
}
.img {
  box-sizing: border-box;
  width: 100%;
  aspect-ratio: 1/1;
  background-size: cover;
  background-position: center;
  border-radius: 5px;
  position: relative;
}

.active {
  pointer-events: none;
  margin: 1px;
  position: absolute;
  border: 3px solid rgba(255, 28, 28, 0.821);
  z-index: 100;
}
.drawer {
  display: flex;
  width: 200%;
  overflow: hidden;
  transition: 0.5s;
  margin: 10px 0px;
}
.rightGrid {
  transform: translateX(100%);
}
.grid {
  width: 100%;
  display: grid;
  height: min-content;
  max-height: 80vh;
  grid-template-columns: 1fr 1fr 1fr;
  box-sizing: border-box;
  overflow-y: scroll;
  gap: 0.5rem;
}
</style>
