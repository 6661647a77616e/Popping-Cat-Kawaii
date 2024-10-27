<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
const isPopped = ref(false)
const popCount = ref(0)
const popSound = ref(null)

const handleKeyPress = (event) => {
  if (event.code === 'Space') {
    event.preventDefault() // Prevent page scrolling
    togglePop()
  }
}

const togglePop = () => {
  isPopped.value = true
  popCount.value++
  
  if (popSound.value) {
    popSound.value.currentTime = 0 // Reset sound to start
    popSound.value.play()
  }
  
  // Automatically return to closed state after 200ms
  setTimeout(() => {
    isPopped.value = false
  }, 200)
}

// Add some encouragement messages based on pop count
const getEncouragement = () => {
  if (popCount.value >= 50) return "NYAA~ You're Amazing! 🌟"
  if (popCount.value >= 30) return "Purrfect Popping! 😺"
  if (popCount.value >= 10) return "Meow-velous! Keep Going! 🐱"
  return "Pop the Kitty! ✨"
}

const starPositions = Array.from({ length: 15 }, () => ({
  left: `${Math.random() * 100}%`,
  top: `${Math.random() * 100}%`,
  delay: `${Math.random() * 5}s`,
  symbol: ['⭐', '🌟', '✨'][Math.floor(Math.random() * 3)]
}))

const getRandomPosition = () => {
  const x = Math.floor(Math.random() * 60) - 30; // Random value between -30 and 30
  const y = Math.floor(Math.random() * 60) - 30; // Random value between -30 and 30
  return `translate(${x}px, ${y}px)`;
}

onMounted(() => {
  // Create audio element
  popSound.value = new Audio('/src/assets/pop.mp3') // Make sure this path matches your audio file location
  // Add keyboard listener
  window.addEventListener('keydown', handleKeyPress)

  onUnmounted(() => {
  // Clean up keyboard listener
  window.removeEventListener('keydown', handleKeyPress)
})
})
</script>

<template>
  <div class="kawaii-container">
    <!-- Cute floating backgrounds -->
    <div class="floating-hearts">
      🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 ✨ 🐾 💕 🌸 
    </div>

    <div class="stars-container">
  <div v-for="(star, index) in starPositions" :key="index" class="floating-star" 
    :style="{ 
      left: star.left, 
      top: star.top,
      animationDelay: star.delay
    }">
    {{ star.symbol }}
  </div>
</div>
    
    <h1 class="kawaii-title">Pop Cat</h1>
    <h2 class="pop-count-big">{{ popCount }}</h2>
    
    <div class="popcat-container" @click="togglePop">
      <div class="popcat-wrapper" :class="{ 'is-popped': isPopped }">
        <img
          :src="isPopped ? '/src/assets/popcat-open.png' : '/src/assets/popcat-closed.png'"
          :alt="isPopped ? 'PopCat Open' : 'PopCat Closed'"
          class="popcat-image"
        />
      </div>
    </div>

    <button @click="togglePop" class="kawaii-button">
      <span class="button-text">Get Poppin'</span>
      <span class="button-paw">🐾</span>
    </button>

    <p class="encouragement">{{ getEncouragement() }}</p>
 
  </div>
</template>

<style scoped>
.kawaii-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  width: 100%;
  padding: 0;
  background: linear-gradient(135deg, #ffe5f0 0%, #fff6e5 100%);
  position: relative;
  overflow: hidden;
  margin: 0;
  box-sizing: border-box;
}


.floating-hearts {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  padding: 20px;  /* Reduced from 40px */
  margin-bottom: 40px; /* Added margin-bottom */
  font-size: 24px;
  text-align: center;
  animation: float 15s linear infinite;
  opacity: 0.5;
}

@keyframes float {
  0% { transform: translateX(100%); }
  100% { transform: translateX(-100%); }
}

.kawaii-title {
  font-family: 'Comic Sans MS', cursive;
  color: #ff6b9d;
  font-size: 3.5rem;
  text-shadow: 3px 3px 0 #fff,
               -1px -1px 0 #fff,
               1px -1px 0 #fff,
               -1px 1px 0 #fff,
               1px 1px 0 #fff;
  margin-top: 60px;
  margin-bottom: 1rem;
  animation: bounce 1s ease infinite;
}

.pop-count-big {
  font-size: 4rem;
  color: #ff8eb4;
  margin-bottom: 1rem;
  font-weight: bold;
}

.popcat-container {
  cursor: pointer;
  padding: 50px;
}

.popcat-wrapper {
  background: rgba(255, 255, 255, 0.8);
  padding: 20px;
  box-shadow: 0 8px 32px rgba(255, 107, 157, 0.2);
  transition: all 0.3s ease;
}

.popcat-wrapper:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 36px rgba(255, 107, 157, 0.3);
}

.popcat-wrapper.is-popped {
  transform: scale(0.95);
}

.popcat-image {
  max-width: 500px;
  transition: transform 0.1s ease-in-out;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.1));
}

.kawaii-button {
  margin-top: 20px;
  padding: 12px 30px;
  font-size: 18px;
  background: linear-gradient(45deg, #ff6b9d, #ff9dbb);
  color: white;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255, 107, 157, 0.3);
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  gap: 10px;
}

.kawaii-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 107, 157, 0.4);
}

.kawaii-button:active {
  transform: translateY(1px);
}

.button-paw {
  font-size: 20px;
  animation: wiggle 1s ease-in-out infinite;
}

.encouragement {
  margin-top: 20px;
  font-size: 1.2rem;
  color: #ff6b9d;
  font-family: 'Comic Sans MS', cursive;
  text-align: center;
  padding: 10px;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 15px;
  box-shadow: 0 4px 15px rgba(255, 107, 157, 0.1);
  transition: transform 0.3s ease; /* Add smooth transition */
  position: relative; /* Add this */
  z-index: 1; /* Ensure it stays above other elements */
}

.pop-count {
  margin-top: 15px;
  font-size: 1.1rem;
  color: #ff8eb4;
  font-weight: bold;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

@keyframes wiggle {
  0%, 100% { transform: rotate(0); }
  25% { transform: rotate(-15deg); }
  75% { transform: rotate(15deg); }
}

.stars-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
}

.floating-star {
  position: absolute;
  font-size: 24px;
  animation: floatAround 10s linear infinite;
  opacity: 0.6;
  pointer-events: none;
}

@keyframes floatAround {
  0% {
    transform: translate(0, 0) rotate(0deg);
  }
  25% {
    transform: translate(50px, 25px) rotate(90deg);
  }
  50% {
    transform: translate(0, 50px) rotate(180deg);
  }
  75% {
    transform: translate(-50px, 25px) rotate(270deg);
  }
  100% {
    transform: translate(0, 0) rotate(360deg);
  }
}

/* Add some variation to the animation for more randomness */
.floating-star:nth-child(2n) {
  animation-duration: 15s;
  animation-direction: reverse;
}

.floating-star:nth-child(3n) {
  animation-duration: 12s;
  animation-timing-function: ease-in-out;
}

.floating-star:nth-child(5n) {
  animation-duration: 18s;
  animation-direction: alternate;
}

</style>