<template>
  <router-view />
</template>

<script>
//Begin vh fix
const documentHeight = () => {
  const doc = document.documentElement;
  var vh = window.innerHeight * 0.01;
  doc.style.setProperty("--doc-vh", `${vh}px`);
};

window.addEventListener("resize", documentHeight);
documentHeight();
//End vh fix
</script>

<style>
:root {
  --filled: #02ffe1;
  --background: #000;
  --fruit: #fff;
  --doc-vh: 0;
}

* {
  margin: 0;
  overscroll-behavior: none;
}

@font-face {
  src: url("@/assets/fonts/TiltNeon.ttf");
  font-family: "TiltNeon";
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: var(--background);

  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
  height: calc(100 * var(--doc-vh));
  width: 100vw;
}
</style>
