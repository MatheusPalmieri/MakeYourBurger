<template>
  <main class="content">
    <section>
      <Navbar :logo="logo" :alt="alt" />

      <router-view />
    </section>

    <Footer />
  </main>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";

export default {
  components: {
    Navbar,
    Footer,
  },
  data() {
    return {
      logo: "/img/logo.png",
      alt: "Make Your Burger",
    };
  },
};
</script>

<style>
* {
  font-family: Helvetica, sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.content {
  min-height: 100dvh;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.container {
  margin: 50px;
  min-height: 250px;
}

h1 {
  text-align: center;
  font: 42px;
  color: #222;
  margin-bottom: 30px;
}
</style>
