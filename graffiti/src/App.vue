<template>

  <div id="app">
    
    
    <AppCursor />

    <PageHome />

    <AppParticles />

  </div>

</template>

<script>
import PageHome from './view/PageHome.vue'
import AppCursor from './components/particles/AppCursor.vue'
import AppParticles from './components/particles/AppParticles.vue'

export default {
  name: 'App',
  components: {
    PageHome,
    AppCursor,
    AppParticles

  }
}
</script>


<style lang="css" module>
html, body {
  margin: 0;
  padding: 0;
}

</style>
