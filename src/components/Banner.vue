<template>
  <div id="banner"><h1>Make Your Burger</h1></div>
</template>

<script>
export default {
  name: "Banner",
};
</script>

<style scoped>
#banner {
  height: 450px;
  background-image: url("/public/img/burger.jpg");
  background-position: 0;
  background-size: cover;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

#banner h1 {
  color: #fff;
  text-align: center;
  font-size: 60px;
  background: #222;
  padding: 20px 40px;
  border-radius: 0 10px 10px 0;
}
</style>
