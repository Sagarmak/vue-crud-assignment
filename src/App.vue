<template>
  <div id="app">
    <Navbar />
    <HomeComponent />
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import HomeComponent from './components/HomeComponent.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    HomeComponent
  }
}
</script>

<style>
*,
*::after,
*::before {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
  box-sizing: border-box;
}

body {
  min-height: 100vh;
  min-width: 100%;
  line-height: 1;
  font-family: 'Roboto', sans-serif;
  font-size: 14px;
  background: #fafafa;
  color: #2b3945;
}


/* Helper */
.container {
  margin: 0 auto;
  max-width: 80%;
  padding: 2rem;
  padding-bottom: 0;
}

/* Flexbox Helper */
.flex {
  display: flex;
}
.flex-column {
  flex-direction: column;
}
.justify-center {
  justify-content: center;
}
.justify-between {
  justify-content: space-between;
}
.align-center {
  align-items: center;
}
.flex-wrap {
  flex-wrap: wrap;
}

.bold {
  font-weight: bold;
}
.title {
  font-size: 1.3rem;
}
.mr-2 {
  margin-right: 10px;
}
button {
  cursor: pointer;
}
.modal {
  z-index: 1;
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: rgb(0, 0, 0, 0.5);
  overflow: auto;
}
.modal-content {
  width: 85%;
  margin: 2rem auto;
  background: #fefefe;
}
</style>
