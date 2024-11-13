<template>
  <div class="particle-menu">
    <input type="range" id="sizeSlider" min="0.01" max="0.2" step="0.01" v-model="particleSize" @input="updateSize" />
    <label for="sizeSlider">Tamanho do Spray</label>
    <input type="color" v-model="particleColor" @input="updateColor" />
    <label for="colorPicker">Cor</label>
  </div>

  <div ref="container" class="particle-container" @mousedown="startSpray" @mouseup="stopSpray" @mousemove="createSpray"></div>
</template>

<script>
import * as THREE from 'three';

export default {
  name: 'AppParticles',
  mounted() {
    this.initScene();
    window.addEventListener('resize', this.handleResize);
  },
  unmounted() {
    window.removeEventListener('resize', this.handleResize);
  },
  data() {
    return {
      spraying: false, // Controla se estamos no modo de spray
      mouseX: 0, // Posição X do mouse no espaço 2D
      mouseY: 0, // Posição Y do mouse no espaço 2D
      particleSize: 0.05, // Tamanho inicial das partículas
      particleColor: '#ff0000', // Cor inicial das partículas
      activeParticles: [] // Array para armazenar partículas ativas
    };
  },
  methods: {
    initScene() {
      // Criar a cena, câmera e renderizador
      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      this.renderer = new THREE.WebGLRenderer({ alpha: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.container.appendChild(this.renderer.domElement);

      // Criar o grupo de partículas
      this.particles = new THREE.Group();
      this.scene.add(this.particles);

      // Posição inicial da câmera
      this.camera.position.z = 5;

      // Iniciar animação
      this.animate();
    },

    startSpray(event) {
      this.spraying = true;
      this.createSpray(event); // Cria partículas imediatamente quando o botão é pressionado
    },

    stopSpray() {
      this.spraying = false;
    },

    createSpray(event) {
      if (!this.spraying) return;

      // Atualizar as posições do mouse
      this.mouseX = (event.clientX / window.innerWidth) * 2 - 1;
      this.mouseY = -(event.clientY / window.innerHeight) * 2 + 1;

      // Usar a posição do mouse diretamente no espaço 3D
      const mouse = new THREE.Vector2(this.mouseX, this.mouseY);

      const mousePosition = new THREE.Vector3(mouse.x , mouse.y + 0.1, 0.5); // Z ajustado para um valor médio
      mousePosition.unproject(this.camera); // Converte para coordenadas no mundo 3D

      // Criar partículas com esferas
      const particleCount = 1;
      const material = new THREE.MeshBasicMaterial({ color: this.particleColor });

      for (let i = 0; i < particleCount; i++) {
        const geometry = new THREE.SphereGeometry(this.particleSize); // Usar o tamanho do spray configurado
        const sphere = new THREE.Mesh(geometry, material);
        
        // Posicionar a esfera em torno da posição do mouse
        sphere.position.set(
          mousePosition.x + Math.random() * 0.01 - 0.01,
          mousePosition.y + Math.random() * 0.01 - 0.01,
          mousePosition.z + Math.random() * 0.01 - 0.01
        );
        
        // Adicionar a esfera ao grupo de partículas e à lista de partículas ativas
        this.particles.add(sphere);
        this.activeParticles.push(sphere);
      }
    },

    updateSize() {
      this.particleSize = parseFloat(this.particleSize);
    },

    animate() {
      requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
    },

    handleResize() {
      const width = window.innerWidth;
      const height = window.innerHeight;

      this.camera.aspect = width / height;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(width, height);
    },
  },
};
</script>

<style scoped>
.particle-container {
  position: relative;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-color: #fff; /* Cor de fundo para simular uma tela */
}
</style>
