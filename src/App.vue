<template lang="pug">
div(id="app")
  div(id="nav")
    .logo.hide.c(@click="openUrl")
      img(src="./assets/demo/logo/banner.jpg")
    .logo.c(@click="openUrl")
      img(src="./assets/demo/logo/logo.jpg" )
    .logo.hide(style="justify-content: flex-end;")
      .txt 因電子設備呈現之差異，本網站內容之顏色、圖片、文字等，僅供參考，實際以色卡為準。
      
    //- .navbar
    //-   //- router-link(to="/") 關於公司
    //-   router-link(to="/demo") 精品Demo
    //-   //- router-link(to="/product") 產品資訊
    //-   //- router-link(to="/case") 實績案例
    //-   //- router-link(to="/aFrame") 360觀看
    //-   //- router-link(to="/three") 3d模型
    //-   //- router-link(to="/nav") 目錄下載
  router-view
  .txt1.unhide(style="width:100%") 因電子設備呈現之差異，本網站內容之顏色、圖片、文字等，僅供參考，實際以色卡為準。
</template>

<script>
export default {
  name: 'App',
  methods: {
    openUrl() {
      window.open(
        'https://www.syuanhuang.com.tw/products_detail/252',
        '_blank'
      );
    },
  },
};
</script>

<style lang="scss">
html,
body {
  overscroll-behavior-y: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.btn {
  padding: 5px 10px;
  background-color: #322821;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}
.c {
  cursor: pointer;
}
.logo {
  flex: 1;
  display: flex;
  align-items: center;
  position: relative;
  height: 100%;
  .txt {
    text-align: end;
  }
  img {
    height: 100%;
    object-fit: contain;
  }
}

#nav {
  display: flex;
  align-items: center;
  height: 10vh;
  padding: 10px 10px;
  justify-content: space-between;
  box-sizing: border-box;
  .navbar {
    display: flex;
    box-sizing: border-box;
  }
  a {
    font-weight: bold;
    color: #2c3e50;
    text-decoration: none;
    padding: 5px;
    margin-left: 30px;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

.fade-enter-active,
.fade-leave {
  transition: 0.9s;
}
.fade-enter,
.fade-leave-active {
  opacity: 0;
  height: 0px;
}
.txt {
  font-size: 12px;
  max-width: 200px;
  text-align: right;
  color: #8a309e;
}
.txt1 {
  font-size: 12px;
  text-align: center;
  color: #8a309e;
  display: none;
}
// hide on mobile device
@media screen and (max-width: 768px) {
  .hide {
    display: none;
  }
  .unhide {
    display: block;
  }
  .logo {
    height: 80%;
  }
}
</style>
