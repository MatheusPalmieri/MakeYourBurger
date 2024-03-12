<template>
  <Banner />
  <div class="container">
    <h1>Make Your <span style="color: #fcba03">Burger</span></h1>

    <BurgerForm />
  </div>
</template>

<script>
import Banner from "@/components/Banner.vue";
import BurgerForm from "@/components/BurgerForm.vue";

export default {
  name: "HomeView",
  components: {
    Banner,
    BurgerForm,
  },
};
</script>
