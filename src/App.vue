<template>
  <section id="app">
    <header>
      <h1 class="hidden">Welcome to Gabriel Chavez Chat App</h1>
      <h2>Gabriel Chavez Chat App with VueCLI, Socket.io and Express</h2>
    </header>
    <!-- Router view is the container that accepts components to render per route -->
    <router-view></router-view>
  </section>
</template>

<style lang="scss">

@import "@/assets/sass/_chat.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header {
  text-align: center;
  background-color: #414833;
  height: 80px;
  padding: 50px;
  color: white;
  box-shadow: 0 2px 10px #ccc;
}

header {
  .hidden { display: none; }
}
header { 
  h2 { padding-top: 20px; padding-bottom: 20px; font-size: 2.5em; margin-bottom: 0.8em;}
}
</style>
