<template>
  <drawer-app/>
</template>

<script>
import DrawerApp from './components/DrawerApp.vue'

export default {
  name: 'App',
  components: {
    DrawerApp
  }
}
</script>

<style>
  body {
    margin: 0;
    padding: 0;
    height: 100vh;
    width: 100%;
    overflow: hidden;
  }

</style>
