<template>
<div id="app">
    <Alert></Alert>
    <Heading></Heading>
    <Navigation></Navigation>
    <router-view/>
</div>
</template>

<script>
// @ is an alias to /src
import Navigation from '@/components/Navigation.vue'
import Alert from '@/components/Alert.vue'
import Heading from '@/components/Heading.vue'

export default {
  name: 'App',
  components: {
    Alert,
    Navigation,
    Heading
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;

  text-decoration: none;
  font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
}

::selection {
  background-color: #c9c9c9;
}

a, button, .closeButton {
    cursor: pointer;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  margin: auto;
  width: 1000px;
}

#nav a.router-link-exact-active {
  background-color: #282C34;
  color: white;
}

@media only screen and (max-width: 1033px) {
    #app {
        width: 100vw;
        padding: 0 20px 0 20px;
    }
}
</style>
