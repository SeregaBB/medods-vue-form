<template>
  <div id="app">
    <RegisterForm />
  </div>
</template>

<script>
import RegisterForm from "./components/RegisterForm.vue";
document.title = "Vue Form";
export default {
  name: "App",
  components: {
    RegisterForm
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #15202b;
  margin: 0;
  width: 100vw;
  background-image: url("./assets/back.jpg");
  padding: 20px 0;
}
body {
  margin: 0;
}
</style>
