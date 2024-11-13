<template>
  <div
    ref="canvasContainer"
    @mousemove="handleMouseMove"
    @click="handleClick"
    class="spray-container"
  >
    <canvas ref="canvas" width="500" height="500"></canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ctx: null,
      particles: [], // Armazena as partículas
    };
  },
  mounted() {
    this.ctx = this.$refs.canvas.getContext('2d');
    this.startAnimation();
  },
  methods: {
    handleMouseMove() {
      // Deixamos esse método vazio já que não está sendo usado, mas pode ser customizado se necessário.
    },
    handleClick(event) {
      const x = event.offsetX;
      const y = event.offsetY;

      // Gerar partículas no ponto clicado
      this.createParticles(x, y);
    },
    createParticles(x, y) {
      const particleCount = 30; // Quantidade de partículas geradas no clique
      console.log(`Gerando ${particleCount} partículas em (${x}, ${y})`);  // Log para debug

      // Gerar partículas
      for (let i = 0; i < particleCount; i++) {
        const angle = Math.random() * Math.PI * 2;  // Aleatório em todas as direções
        const speed = Math.random() * 3 + 2;  // Velocidade variada
        const size = Math.random() * 3 + 2;   // Tamanho variado
        const opacity = 1;  // Começam com opacidade total

        // Partícula com velocidade para baixo (efeito de gravidade)
        const particle = {
          x,
          y,
          angle,
          speed,
          size,
          opacity,
          gravity: 0.1, // Efeito gravitacional
          lifetime: 100, // Vida útil da partícula (em ciclos de animação)
        };

        this.particles.push(particle);

        // Log para verificar os dados da partícula
        console.log('Partícula criada:', particle);
      }
    },
    startAnimation() {
      const animate = () => {
        this.ctx.clearRect(0, 0, this.$refs.canvas.width, this.$refs.canvas.height);

        // Atualizar partículas
        this.particles.forEach((particle, index) => {
          // Atualizar a posição da partícula
          particle.x += Math.cos(particle.angle) * particle.speed;
          particle.y += Math.sin(particle.angle) * particle.speed + particle.gravity; // Simula a queda (gravidade)
          particle.opacity -= 0.01; // Gradualmente desaparecer

          // Diminuir a velocidade gradualmente para simular a dissipação
          particle.speed *= 0.99;

          // Desenhar a partícula
          this.ctx.beginPath();
          this.ctx.arc(particle.x, particle.y, particle.size, 0, Math.PI * 2);
          this.ctx.fillStyle = `rgba(255, 255, 255, ${Math.max(0, particle.opacity)})`; // Cor com opacidade
          this.ctx.fill();

          // Remover partículas com opacidade muito baixa ou tempo de vida expirado
          if (particle.opacity <= 0 || particle.lifetime <= 0) {
            this.particles.splice(index, 1);
          } else {
            particle.lifetime--;  // Diminuir o tempo de vida da partícula
          }
        });

        requestAnimationFrame(animate);
      };

      animate();
    },
  },
};
</script>

<style scoped>
.spray-container {
  position: relative;
  cursor: pointer;
  display: inline-block;
  width: 500px;
  height: 500px;
  border: 1px solid #ccc;
  z-index: 3;
}

canvas {
  width: 100%;
  height: 100%;
  display: block; /* Impede o canvas de ter espaços extras abaixo */
}
</style>
