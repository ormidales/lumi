<template>
  <div class="ball">
    <div class="eyes">
      <div class="eye"></div>
      <div class="eye"></div>
    </div>
    <div class="mouth"></div>
  </div>
</template>

<style scoped>
.ball {
  position: absolute;
  bottom: 60px;
  right: 60px;
  width: 100px;
  height: 100px;
  z-index: 8;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(50, 50, 50, 0.5);
  backdrop-filter: blur(10px);
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.ball:hover {
  background-color: rgba(50, 50, 50, 0.8);
  transform: scale(1.1);
}

.ball:active {
  transform: scale(0.9);
}
</style>
