<template lang="pug">
  .home
    img(src="../assets/logo.png")
    HelloWorld(msg="qwe")

</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
};
</script>

<style lang="scss" scoped>
.home {
  height: 90vh;
}
</style>
