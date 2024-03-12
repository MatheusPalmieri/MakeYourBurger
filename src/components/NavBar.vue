<template>
  <nav id="nav">
    <router-link to="/" id="logo"><img :src="logo" :alt="alt" /></router-link>
    <router-link to="/" class="link">Home</router-link>
    <span> | </span>
    <router-link to="/requests" class="link">Requests</router-link>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  props: ["logo", "alt"],
};
</script>

<style scoped>
#nav {
  background: #222;
  border-bottom: 1px solid #111;
  padding: 15px 50px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  box-shadow: -0px 20px 20px 0px rgba(0, 0, 0, 0.2);
}

#nav #logo {
  margin: auto;
  margin-left: 0;
}

#logo img {
  width: 40px;
  height: 40px;
  transition: 0.2s;
}

@keyframes spring {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1.1);
  }
}

#logo img:hover {
  animation: normal 0.5s spring;
  transform: scale(1.1);
}

#nav span {
  color: #fcba03;
  opacity: 0.5;
}

#nav .link {
  text-decoration: none;
  color: #fcba03;
  margin: 0 15px;
  transition: 0.3s;
  position: relative;
}

#nav .link::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: -2px;
  width: 60%;
  height: 1px;
  background: #fff;
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s;
}

#nav .link:hover {
  color: #fff;
}

#nav .link:hover::after {
  transform: scaleX(1);
  transform-origin: left;
  transition: transform 0.3s;
}
</style>
