<template>
  <div class="graffiti-container">
    <h1 class="graffiti-text">{{ text }}</h1>
  </div>
</template>

<script>
export default {
  name: "GraffitiTitle",
  data() {
    return {
      text: "O graffiti", 
      messages: [
        
        "A arte nas ruas que desafia",
        "O sistema e colore a resistência."
      ],
      currentIndex: 0,
    };
  },
  mounted() {
    
    setInterval(this.changeMessage, 2000);
  },
  methods: {
    changeMessage() {
      
      this.currentIndex = (this.currentIndex + 1) % this.messages.length;
      this.text = this.messages[this.currentIndex];
    },
  },
};
</script>

<style scoped>
@font-face {
  font-family: 'GraffitiFont';
  src: url('@/assets/fonts/GraffitiFont.ttf') format('truetype');
}

.graffiti-container {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background-image: url('@/assets/images/background.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  height: 100vh;
  z-index: 1;
}

.graffiti-text {
  font-family: 'GraffitiFont', sans-serif;
  font-size: 8rem;
  color: #f50;
  text-shadow: 
    2px 2px 0px #000, 
    4px 4px 0px #000, 
    6px 6px 5px rgba(0, 0, 0, 0.5);
  position: relative;
  z-index: 1; 
  letter-spacing: 5px;
}

.graffiti-text::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/images/splatter.png');
  background-size: cover;
  opacity: 0.3;
  z-index: 2; 
}

</style>
