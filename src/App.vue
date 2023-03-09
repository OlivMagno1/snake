<template>
  <router-view />
</template>

<style>
:root {
  --filled: #44633f;
  --background: #3f4b3b;
  --empty: #5a9367;
  --fruit: #6a0136;
  --specialFruit: #4c1a57;
}

* {
  margin: 0;
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
  height: 100vh;
  width: 100vw;
}
</style>
