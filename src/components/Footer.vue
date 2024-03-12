<template>
  <footer id="footer">
    <p>Make Your Burger &copy; {{ new Date().getFullYear() }}</p>
  </footer>
</template>

<script>
export default {
  name: "Footer",
};
</script>

<style scoped>
#footer {
  width: 100%;
  height: 200px;
  background: #222;
  border-top: 2px solid #111;
  font-size: 20px;
  font-weight: 600;
  color: #fcba03;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
