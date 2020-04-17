<template lang="pug">
  .prodoct
    .drawer
      .search
        input(v-model="search")
        .btn search
      .catlogs
        .catlog
          .catlog-items
            .item(v-for="(item ,index) in catlogs") 
              .item-title(@click="clickItem(index)") {{item.item}}
              transition-group(name="fade")
                .item-detail(v-for="(detail, dindex) in item.detail" v-if="itemOpen == index", :key="`${index}_${dindex}`" @click="moveTotexture") {{detail}}
        .catlog
          .btn(@click="moveTotexture(-1)") back
          .textureItems
            .texture(v-for="(tex,index) in texture")
              .textImg(@click="clickTexture(index)" :class="{ active: choosenTextureStyle(index)}", :style="{backgroundImage: 'url('+ require(`../assets/floor_texture/texture${index}.jpeg`) +')'}")
                .cover {{index}}


    .demo(:style="{backgroundImage: 'url('+ require(`../assets/floor_example/image${choosenTexture}.jpeg`) +')'}")
      

</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
  computed: {
    isActive(index) {
      if (index == this.choosenTexture) {
        return true;
      } else {
        return false;
      }
    },
  },
  mounted() {
    window.onresize = () => {
      this.moveTotexture(-1);
    };
  },
  methods: {
    choosenTextureStyle(index) {
      if (index == this.choosenTexture) {
        return true;
      } else {
        return false;
      }
    },
    clickTexture(index) {
      this.choosenTexture = index;
    },
    clickItem(index) {
      console.log(index);
      this.itemOpen = index;
    },
    moveTotexture(dis) {
      var tg = document.querySelector(".catlogs");
      if (dis == -1) {
        tg.style.WebkitTransform = `translateX(0px)`;
      } else {
        tg.style.WebkitTransform = `translateX(-${tg.clientWidth / 2}px)`;
      }
    },
  },
  data() {
    return {
      search: "",
      itemOpen: 0,
      choosenTexture: 0,
      texture: [0, 1, 2, 3, 4, 5, 6],
      catlogs: [
        {
          item: "大理石紋",
          detail: ["米瑞雲石", "亞卡拉石", "星苑", "琥珀石", "多倫石", "帕斯"],
        },
        {
          item: "石紋地、壁磚",
          detail: [
            "米瑞格羅石",
            "米瑞哈伯石",
            "卡多索",
            "哈比",
            "里亞托",
            "拉爾",
            "泰拉",
          ],
        },
        {
          item: "木紋磚",
          detail: [
            "杉木",
            "西西里",
            "格拉夫",
            "杉木",
            "西西里",
            "格拉夫",
            "杉木",
            "西西里",
            "格拉夫",
          ],
        },
        { item: "金屬磚", detail: ["米瑞格羅石", "堤坦", "芝加哥"] },
        {
          item: "花磚/異形磚",
          detail: [
            "米爾",
            "紐約",
            "巴黎地鐵磚",
            "蜂巢",
            "星光",
            "尼斯",
            "摩洛哥",
            "底特律",
            "赫曼",
          ],
        },
        {
          item: "戶外厚磚",
          detail: ["磐石", "卡多索厚磚", "波爾", "巴賽隆納"],
        },
        { item: "外牆磚", detail: ["德國射出磚", "日本小澤琉璃釉馬賽克"] },
      ],
    };
  },
};
</script>

<style lang="scss" scoped>
.prodoct {
  height: 90vh;
  display: flex;
  padding: 20px 30px;
  box-sizing: border-box;
  overflow: hidden;
  .demo {
    width: 100%;
    height: 100%;
    background-image: url("../assets/floor_example/image0.jpeg");
    background-position: center center;
    border-radius: 10px;
    box-shadow: 10px 10px 12px #24242466;
    transition: 0.3s;
  }
}

.drawer {
  width: 30%;
  padding: 10px;
  box-sizing: border-box;
  overflow-y: scroll;
  overflow-x: hidden;

  .search {
    display: flex;
    input {
      flex: 1;
    }
    .btn {
      margin: 0px 5px;
    }
  }
  .catlog {
    transition: 0.5s;
    width: 100%;
    box-sizing: border-box;
    padding: 10px 5px;
  }
}

.catlogs {
  display: flex;
  width: 200%;
  overflow: hidden;
  transition: 0.5s;
  margin: 10px 0px;
}
.catlog-items {
  display: flex;
  flex-direction: column;
  border-radius: 5px;
  overflow: hidden;
  .item {
    flex: 1;
    text-align: left;
    box-sizing: border-box;
    color: white;
    cursor: pointer;
    overflow: hidden;
    transition: 0.5s;

    &-title {
      padding: 10px 10px;
      background: #503d30;
      &:hover {
        background: #322821;
      }
    }
    &-detail {
      height: auto;
      background-color: rgb(255, 255, 255);
      padding: 15px 10px;
      color: black;
      &:hover {
        background-color: rgb(238, 238, 238);
      }
    }
  }
}

.textureItems {
  display: flex;
  flex-wrap: wrap;
  .texture {
    width: 50%;
    height: 150px;
    box-sizing: border-box;
    padding: 10px;
    .active {
      border: 3px solid rgba(255, 28, 28, 0.486);
    }
    .textImg {
      cursor: pointer;
      width: 100%;
      height: 100%;
      background-image: url("../assets/floor_texture/texture0.jpeg");
      border-radius: 5px;
      box-shadow: 6px 8px 12px rgb(202, 202, 202);
      position: relative;
      box-sizing: border-box;
      .cover {
        position: absolute;
        border-radius: 5px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.582);
        opacity: 0;
        transition: 0.3s;
      }
      &:hover {
        .cover {
          opacity: 1;
        }
      }
    }
  }
}
</style>
