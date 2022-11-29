<template lang="pug">
.demoPage
  .outer(style="")
    .tabs
      .tab(@click="selectStyleTab('style')"  :class="{ activeTab: selectedStyleTab == 'style'}") 版型
      .tab(@click="selectStyleTab('texture')"  :class="{ activeTab: selectedStyleTab == 'texture'}") 色號
    .drawer
      .grid
        .textImg(v-for="style in styles")
          .img(@click="selectStyle(style)" :class="{ active: choosenStyle(style)}" :style="{backgroundImage: 'url('+ require(`../assets/demo/style/${style}.jpg`) +')'}")
          .text {{style}}
      .w(style="width:10px")
      .grid.gridRight
        .textImg(v-for="texture in textures")
          .img(@click="selectTexture(texture)" :class="{ active: choosenTexture(texture)}" :style="{backgroundImage: 'url('+ require(`../assets/demo/texture/${texture}.jpg`) +')'}"  )
          .text {{texture}}
      //- .activeBorder(:class="{ active: choosenTextureStyle(texture)}")
  //- .bigImg(v-if="selectedTexture && selectedStyle" :style="{backgroundImage: 'url('+ require(`../assets/demo/render/${this.selectedStyle}/臥室-${this.selectedStyle}_${this.selectedTexture}.jpg`) +')'}"  )
  a-scene.sceneStyle(device-orientation-permission-ui="enabled: false" vr-mode-ui="enabled: false" renderer="antialias: true" embedded=true v-if="selectedTexture && selectedStyle")
    a-sky(:src="require(`../assets/demo/render/${this.selectedStyle}/臥室-${this.selectedStyle}_${this.selectedTexture}.jpg`)" rotation="0 -90 0")
    .monitor 
      .text 色號：{{selectedStyle}}
      .text 版型：{{selectedTexture}}
</template>

<script>
import * as THREE from 'aframe';

export default {
  name: 'Demo',
  data() {
    return {
      selectedTexture: 'U10',
      selectedStyle: 'W',
      selectedStyleTab: 'style',
      styles: [
        'F',
        'F-1',
        'F-2',
        'F-4',
        'H',
        'H-1',
        'H-2',
        'U1',
        'U2',
        'U3',
        'U4',
        'U5',
        'M7',
        'M8',
        'W',
        'X6',
        'X6-1',
      ],
      textures: [
        'U10',
        'U20',
        'U30',
        'U40',
        'U50',
        'U60',
        'U70',
        'U80',
        'K01',
        'K02',
        'K03',
        'K04',
        'K05',
        'K06',
        'K07',
        'K08',
        'K09',
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
    selectStyleTab(tabName) {
      this.selectedStyleTab = tabName;
      if (tabName == 'texture') {
        this.moveTotexture();
      } else {
        this.moveTotexture(-1);
      }
    },
    selectTexture(texture) {
      this.selectedTexture = texture;
      this.init();
    },
    selectStyle(style) {
      this.selectedStyle = style;
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

.tabs {
  display: flex;
  width: 100%;
  .tab {
    width: 50%;
    text-align: center;
    padding: 10px 0;
    cursor: pointer;
    &.activeTab {
      border: 1px solid rgb(156, 156, 156);
      border-radius: 5px;
      background-color: #f5f5f5;
    }
  }
}

.sceneStyle {
  margin: 10px;
  box-sizing: border-box;
  border-radius: 10px;
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
  position: relative;
  cursor: pointer;
}
.img {
  box-sizing: border-box;
  width: 100%;
  aspect-ratio: 2/3;
  background-size: cover;
  background-position: center;
  border-radius: 5px;
  position: relative;
  box-shadow: 0 0 15px 0 rgba(0, 0, 0, 0.268);
}

.active {
  pointer-events: none;
  margin: 1px;
  border: 3px solid rgba(255, 28, 28, 0.821);
}
.outer {
  display: flex;
  flex-direction: column;
  min-width: 15vw;
  max-width: 150px;
  position: relative;
  overflow-x: hidden;
}
.drawer {
  display: flex;
  width: 200%;
  height: 100%;
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
  height: max-content;
  max-height: 100%;
  grid-template-columns: 1fr 1fr;
  box-sizing: border-box;
  overflow-y: scroll;
  gap: 0.5rem;
}

.monitor {
  position: absolute;
  text-align: left;
  font-size: 0.8rem;
  top: 0;
  left: 0;
  background-color: #aeaeae6d;
  z-index: 100;
  padding: 0.5rem;
}
</style>
