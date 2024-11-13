<template>
  <div class="custom-cursor"></div>
</template>

<script>
document.addEventListener('mousemove', (event) => {
  const cursor = document.querySelector('.custom-cursor');
  if (cursor) {
    cursor.style.left = `${event.clientX}px`;
    cursor.style.top = `${event.clientY}px`;
  }
});

</script>

<style scoped>
.custom-cursor {
  position: fixed;
  width: 150px; 
  height: 150px;
  background-image: url('@/assets/images/spray-can.png');
  background-size: contain;
  background-repeat: no-repeat;
  pointer-events: none;
  z-index: 9999;
  transform: translate(-50%, -50%); 
  display: block;
}

body {
  cursor: none; 
}

.custom-cursor.active {
  display: block;
}
</style>



