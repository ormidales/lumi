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

const emit = defineEmits(['eye-click']);

const leftEyeStyle = ref({});
const rightEyeStyle = ref({});
const isLeftEyeClosed = ref(false);
const isRightEyeClosed = ref(false);

const tooltipMessage = ref('');
const showTooltip = ref(false);
const isDizzy = ref(false);
let tooltipInterval = null;

const lastAngle = ref(null);
const totalAngle = ref(0);
let movementTimeout = null;

const messages = [
  "Psst, cliquez sur moi !",
  "Je peux vous aider ?",
  "Vous cherchez quelque chose ?",
  "J'ai plein d'infos pour vous.",
  "N'hésitez pas à me solliciter.",
];

const handleEyeClick = (eye) => {
  if (eye === 'left') {
    isLeftEyeClosed.value = true;
    setTimeout(() => {
      isLeftEyeClosed.value = false;
    }, 300);
  } else {
    isRightEyeClosed.value = true;
    setTimeout(() => {
      isRightEyeClosed.value = false;
    }, 300);
  }
  emit('eye-click');
};

const moveEyes = (event) => {
  if (props.isCardHovered || isDizzy.value) return;

  const ball = event.currentTarget;
  const rect = ball.getBoundingClientRect();
  const mouseX = event.clientX - rect.left;
  const mouseY = event.clientY - rect.top;

  const leftEyeEl = ball.querySelector('.eye:first-child');
  const rightEyeEl = ball.querySelector('.eye:last-child');

  if (leftEyeEl && rightEyeEl) {
    const leftRect = leftEyeEl.getBoundingClientRect();
    const rightRect = rightEyeEl.getBoundingClientRect();

    const leftCenterX = leftRect.left + leftRect.width / 2 - rect.left;
    const leftCenterY = leftRect.top + leftRect.height / 2 - rect.top;
    const rightCenterX = rightRect.left + rightRect.width / 2 - rect.left;
    const rightCenterY = rightRect.top + rightRect.height / 2 - rect.top;

    let leftX = (mouseX - leftCenterX) / 5;
    let leftY = (mouseY - leftCenterY) / 5;
    let rightX = (mouseX - rightCenterX) / 5;
    let rightY = (mouseY - rightCenterY) / 5;

    const maxMove = 7.5;

    const distLeft = Math.sqrt(leftX * leftX + leftY * leftY);
    if (distLeft > maxMove) {
      leftX = (leftX / distLeft) * maxMove;
      leftY = (leftY / distLeft) * maxMove;
    }

    const distRight = Math.sqrt(rightX * rightX + rightY * rightY);
    if (distRight > maxMove) {
      rightX = (rightX / distRight) * maxMove;
      rightY = (rightY / distRight) * maxMove;
    }

    leftEyeStyle.value = {
      transform: `translate(${leftX}px, ${leftY}px)`,
      transition: 'transform 0.1s ease-out'
    };
    rightEyeStyle.value = {
      transform: `translate(${rightX}px, ${rightY}px)`,
      transition: 'transform 0.1s ease-out'
    };
  }


  const centerX = rect.width / 2;
  const centerY = rect.height / 2;

  const angle = Math.atan2(mouseY - centerY, mouseX - centerX);
  if (lastAngle.value !== null) {
    let delta = angle - lastAngle.value;
    if (delta > Math.PI) delta -= 2 * Math.PI;
    else if (delta < -Math.PI) delta += 2 * Math.PI;
    totalAngle.value += delta;
  }
  lastAngle.value = angle;

  clearTimeout(movementTimeout);
  movementTimeout = setTimeout(() => {
    totalAngle.value = 0;
    lastAngle.value = null;
  }, 500);

  if (Math.abs(totalAngle.value) >= (10 * Math.PI)) {
    isDizzy.value = true;
    totalAngle.value = 0;
    lastAngle.value = null;
    clearTimeout(movementTimeout);
  }
};

const resetEyesAndGesture = () => {
  if (props.isCardHovered) return;
  leftEyeStyle.value = {
    transform: 'translate(0, 0)',
    transition: 'transform 0.3s ease-in-out',
  };
  rightEyeStyle.value = {
    transform: 'translate(0, 0)',
    transition: 'transform 0.3s ease-in-out',
  };
  clearTimeout(movementTimeout);
  totalAngle.value = 0;
  lastAngle.value = null;
};

const startTooltipCycle = () => {
  if (props.isCardVisible || isDizzy.value) return;
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

watch(isDizzy, (newVal) => {
  if (newVal) {
    stopTooltipCycle();
    setTimeout(() => {
      isDizzy.value = false;
    }, 2000);
  } else {
    if (!props.isCardVisible) {
      setTimeout(startTooltipCycle, 2000);
    }
  }
});

watch(() => props.isCardHovered, (isHovering) => {
  if (isHovering) {
    leftEyeStyle.value = {
      transform: 'translate(-10px, -10px)',
      transition: 'transform 0.3s ease-in-out',
    };
    rightEyeStyle.value = {
      transform: 'translate(-10px, -10px)',
      transition: 'transform 0.3s ease-in-out',
    };
  } else {
    resetEyesAndGesture();
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
  clearTimeout(movementTimeout);
});
</script>

<template>
  <div class="wrapper">
    <div class="tooltip" :class="{ 'visible': showTooltip && !isDizzy }">
      {{ tooltipMessage }}
    </div>
    <div class="ball" @mousemove="moveEyes" @mouseleave="resetEyesAndGesture" :class="{ 'dizzy': isDizzy }">
      <div class="eyes">
        <div class="eye" :class="{ 'closed': isLeftEyeClosed }" @click.stop="handleEyeClick('left')">
          <div v-if="!isDizzy" class="pupil" :style="leftEyeStyle"></div>
          <div v-else class="dizzy-eye">
            <div class="dizzy-line1"></div>
            <div class="dizzy-line2"></div>
          </div>
        </div>
        <div class="eye" :class="{ 'closed': isRightEyeClosed }" @click.stop="handleEyeClick('right')">
          <div v-if="!isDizzy" class="pupil" :style="rightEyeStyle"></div>
          <div v-else class="dizzy-eye">
            <div class="dizzy-line1"></div>
            <div class="dizzy-line2"></div>
          </div>
        </div>
      </div>
      <div class="mouth" :class="{ 'talking': showTooltip && !isDizzy, 'dizzy-mouth': isDizzy }"></div>
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

.ball.dizzy {
  animation: dizzy-spin 2s ease-in-out;
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
  position: relative;
}

.eye.closed::after {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 20px;
  height: 2px;
  background: #2c3e50;
  transform: translate(-50%, -50%);
  border-radius: 1px;
}

.pupil {
  width: 15px;
  height: 15px;
  background-color: #2c3e50;
  border-radius: 50%;
}

.dizzy-eye {
  position: relative;
  width: 18px;
  height: 18px;
  animation: dizzy-eyes-spin 2s ease-in-out;
}

.dizzy-eye .dizzy-line1,
.dizzy-eye .dizzy-line2 {
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 3px;
  background-color: #2c3e50;
  border-radius: 2px;
}

.dizzy-eye .dizzy-line1 {
  transform: translateY(-50%) rotate(45deg);
}

.dizzy-eye .dizzy-line2 {
  transform: translateY(-50%) rotate(-45deg);
}

.mouth {
  width: 35px;
  height: 5px;
  background-color: rgba(255, 255, 255, 1);
  border-radius: 0 0 15px 15px;
  transition: all 0.2s ease-in-out;
}

.mouth.dizzy-mouth {
  width: 25px;
  height: 25px;
  border: 3px solid white;
  background: transparent;
  border-radius: 50%;
  margin-top: -5px;
  animation: dizzy-mouth-shape 2s ease-in-out;
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

@keyframes dizzy-spin {
  0% {
    transform: rotate(0deg) scale(1);
  }

  25% {
    transform: rotate(15deg) translateX(5px);
  }

  50% {
    transform: rotate(-15deg) translateX(-5px);
  }

  75% {
    transform: rotate(10deg) translateX(5px);
  }

  100% {
    transform: rotate(0deg) scale(1);
  }
}

@keyframes dizzy-eyes-spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

@keyframes dizzy-mouth-shape {
  0% {
    border-radius: 50%;
    transform: rotate(0deg);
  }

  50% {
    border-radius: 10% 50%;
    transform: rotate(180deg) scale(0.8);
  }

  100% {
    border-radius: 50%;
    transform: rotate(360deg);
  }
}
</style>
