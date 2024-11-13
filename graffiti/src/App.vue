<template>
  <div id="app">
    <AppCursor />

    <!-- PageHome ficará acima do AppParticles -->
    <PageHome />
    
    <!-- AppParticles ficará abaixo do PageHome -->
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

#app {
  position: relative; /* Necessário para usar z-index nos elementos filhos */
}

PageHome {
  position: absolute; /* Definindo o PageHome como absoluto */
  z-index: 2; /* Garante que o PageHome fique acima do AppParticles */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

AppParticles {
  position: absolute; /* Definindo o AppParticles como absoluto */
  z-index: 1; /* Fica abaixo do PageHome */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
