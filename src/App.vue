<template lang="pug">
  div(id="app")
    div(id="nav")
      .logo
        img(src="./assets/logoDesign.png")
        h3(contenteditable="true") 地板精品
      .navbar
        router-link(to="/") 關於公司
        router-link(to="/news") 最新消息
        router-link(to="/product") 產品資訊
        router-link(to="/case") 實績案例
        router-link(to="/store") 門市位置
        router-link(to="/three") 聯絡我們
        router-link(to="/nav") 目錄下載
    router-view
</template>

<style lang="scss">
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

.logo {
  display: flex;
  align-items: center;
  img {
    margin-right: 10px;
    height: 29px;
    object-fit: cover;
  }
  h3 {
    margin: 0;
  }
}

#nav {
  display: flex;
  align-items: center;
  height: 10vh;
  padding: 20px 30px;
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
</style>
