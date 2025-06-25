<script setup>
import { ref, watch, onUnmounted } from 'vue';

const props = defineProps({
  isCardHovered: {
    type: Boolean,
    default: false,
  },
  isCardVisible: {
    type: Boolean,
    default: false,
  },
});

const eyeStyle = ref({});
const tooltipMessage = ref('');
const showTooltip = ref(false);
let tooltipInterval = null;

const messages = [
  "Psst, cliquez sur moi !",
  "Je peux vous aider ?",
  "Vous cherchez quelque chose ?",
  "J'ai plein d'infos pour vous.",
  "N'hésitez pas à me solliciter.",
];

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

const startTooltipCycle = () => {
  if (props.isCardVisible) return;
  if (tooltipInterval) clearInterval(tooltipInterval);

  let currentIndex = -1;

  const showNextMessage = () => {
    currentIndex = (currentIndex + 1) % messages.length;
    tooltipMessage.value = messages[currentIndex];
    showTooltip.value = true;

    setTimeout(() => {
      showTooltip.value = false;
    }, 5000);
  };

  showNextMessage();
  tooltipInterval = setInterval(showNextMessage, 10000);
};

const stopTooltipCycle = () => {
  if (tooltipInterval) {
    clearInterval(tooltipInterval);
    tooltipInterval = null;
  }
  showTooltip.value = false;
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

watch(() => props.isCardVisible, (isVisible) => {
  if (isVisible) {
    stopTooltipCycle();
  } else {
    setTimeout(startTooltipCycle, 2000);
  }
}, { immediate: true });

onUnmounted(() => {
  stopTooltipCycle();
});
</script>

<template>
  <div class="wrapper">
    <div class="tooltip" :class="{ 'visible': showTooltip }">
      {{ tooltipMessage }}
    </div>
    <div class="ball" @mousemove="moveEyes" @mouseleave="resetEyes">
      <div class="eyes">
        <div class="eye">
          <div class="pupil" :style="eyeStyle"></div>
        </div>
        <div class="eye">
          <div class="pupil" :style="eyeStyle"></div>
        </div>
      </div>
      <div class="mouth" :class="{ 'talking': showTooltip }"></div>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  position: absolute;
  bottom: 60px;
  right: 60px;
  z-index: 8;
  width: 100px;
  height: 100px;
}

.tooltip {
  width: 150px;
  position: absolute;
  bottom: 110px;
  left: 50%;
  transform: translateX(-50%) translateY(10px);
  padding: 8px 12px;
  background-color: rgba(50, 50, 50, 0.8);
  backdrop-filter: blur(5px);
  color: rgba(255, 255, 255, 1);
  border-radius: 8px;
  font-size: 10px;
  font-weight: 500;
  opacity: 0;
  transition: opacity 0.4s ease, transform 0.4s ease;
  pointer-events: none;
}

.tooltip.visible {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

.tooltip::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: rgba(50, 50, 50, 0.8) transparent transparent transparent;
}

.ball {
  width: 100px;
  height: 100px;
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

.ball:hover {
  transform: scale(1.1);
}

.ball:active {
  transform: scale(0.9);
}

.eyes {
  display: flex;
  gap: 16px;
  margin-bottom: 8px;
}

.eye {
  width: 30px;
  height: 30px;
  background-color: rgba(255, 255, 255, 1);
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
  background-color: rgba(255, 255, 255, 1);
  border-radius: 0 0 15px 15px;
  transition: all 0.2s ease-in-out;
}

.mouth.talking {
  animation: talk 0.3s infinite ease-in-out;
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

@keyframes talk {

  0%,
  100% {
    height: 5px;
    border-radius: 0 0 15px 15px;
  }

  50% {
    height: 12px;
    border-radius: 8px 8px 5px 5px;
  }
}
</style>
