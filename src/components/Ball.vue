<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  isCardHovered: {
    type: Boolean,
    default: false,
  },
});

const eyeStyle = ref({});

const moveEyes = (event) => {
  if (props.isCardHovered) return;

  const ball = event.currentTarget;
  const rect = ball.getBoundingClientRect();
  const mouseX = event.clientX - rect.left;
  const mouseY = event.clientY - rect.top;

  const x = (mouseX / rect.width - 0.5) * 25;
  const y = (mouseY / rect.height - 0.5) * 25;

  eyeStyle.value = {
    transform: `translate(${x}px, ${y}px)`,
    transition: 'transform 0.1s ease-out'
  };
};

const resetEyes = () => {
  if (props.isCardHovered) return;

  eyeStyle.value = {
    transform: 'translate(0, 0)',
    transition: 'transform 0.3s ease-in-out',
  };
};

watch(() => props.isCardHovered, (isHovering) => {
  if (isHovering) {
    eyeStyle.value = {
      transform: 'translate(-10px, -10px)',
      transition: 'transform 0.3s ease-in-out',
    };
  } else {
    resetEyes();
  }
});
</script>

<template>
  <div class="ball" @mousemove="moveEyes" @mouseleave="resetEyes">
    <div class="eyes">
      <div class="eye">
        <div class="pupil" :style="eyeStyle"></div>
      </div>
      <div class="eye">
        <div class="pupil" :style="eyeStyle"></div>
      </div>
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
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: rgba(50, 50, 50, 0.5);
  backdrop-filter: blur(10px);
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.ball:hover .mouth {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 4px solid white;
  background-color: transparent;
}

.ball:active {
  transform: scale(0.9);
}

.eyes {
  display: flex;
  gap: 20px;
  margin-bottom: 12px;
}

.eye {
  width: 30px;
  height: 30px;
  background-color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  animation: blink 4s infinite ease-in-out;
}

.pupil {
  width: 15px;
  height: 15px;
  background-color: #2c3e50;
  border-radius: 50%;
}

.mouth {
  width: 35px;
  height: 5px;
  background-color: white;
  border-radius: 0 0 15px 15px;
  transition: all 0.2s ease-in-out;
}

@keyframes blink {

  0%,
  90%,
  100% {
    transform: scaleY(1);
  }

  95% {
    transform: scaleY(0.1);
  }
}
</style>
