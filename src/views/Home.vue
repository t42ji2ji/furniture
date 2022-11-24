<template lang="pug">
.home
  .texts
    .title 地板精品
    .subtitle 跟你接觸最親密的家具
  .showcase
    .show(v-for="x in showdata" :style="{backgroundImage: 'url(' + x + ')'}")

</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import { gsap } from 'gsap';

export default {
  name: 'Home',
  components: {
    HelloWorld,
  },
  data() {
    return {
      showdata: [
        'https://images.unsplash.com/photo-1567101692882-6630684230bf?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60',
        'https://images.unsplash.com/photo-1550921082-c282cdc432d6?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60',
        'https://images.unsplash.com/photo-1583700852632-570667e5fa52?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60',
      ],
    };
  },
  mounted() {
    gsap.from('.show', 1, { y: -40, opacity: 0, stagger: 0.2 });
  },
  methods: {},
};
</script>

<style lang="scss" scoped>
.home {
  height: 90vh;
  display: flex;
  // background-image: url("../assets/360image/t3.jpg");
  background-position: center center;
  background-size: cover;
  position: relative;
}
.showcase {
  display: flex;
  position: absolute;
  bottom: 5%;
  right: 1%;
  .show {
    margin: 5px 10px;
    width: 250px;
    height: 400px;
    background-color: #fff;
    border-radius: 10px;
    background-position: center center;
    background-size: cover;
    position: relative;
    box-shadow: 2px 2px 12px rgb(195, 195, 195);
    position: relative;
    overflow: hidden;
    &::after {
      content: '';
      position: absolute;
      left: 0;
      top: 0;
      width: 250px;
      height: 400px;
      background-color: black;
      opacity: 0;
      transition: 0.3s;
    }
    &:hover {
      &::after {
        content: '';
        background-color: black;
        opacity: 20%;
      }
    }
  }
}
.texts {
  animation: slide-in-right 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) both;
  position: absolute;
  top: 20%;
  left: 10%;
  text-shadow: 2px 2px 12px rgb(161, 161, 161);

  .title {
    font-weight: bold;
    font-size: 4rem;
  }

  .subtitle {
    letter-spacing: 10px;
    margin-left: 10px;
  }
}
@keyframes slide-in-right {
  0% {
    transform: translateX(100px);
    opacity: 0;
  }
  100% {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>
