<script setup>
import { ref, computed } from 'vue'

const weight = ref(70)
const reps = ref(5)

const oneRepMax = computed(() => {
  if (!weight.value || !reps.value) return 0
  if (reps.value === 1) return weight.value
  return weight.value * (1 + reps.value / 30)
})

const funWeights = [
  { min: 0, max: 40, text: "a domestic cat 🐈", icon: "🐈" },
  { min: 41, max: 70, text: "a large wolfhound 🐕", icon: "🐕" },
  { min: 71, max: 90, text: "a sack of potatoes 🥔", icon: "🥔" },
  { min: 91, max: 110, text: "an adult wild boar 🐗", icon: "🐗" },
  { min: 111, max: 140, text: "an adult panda 🐼", icon: "🐼" },
  { min: 141, max: 180, text: "a Vespa scooter 🛵", icon: "🛵" },
  { min: 181, max: 250, text: "an adult lion 🦁", icon: "🦁" },
  { min: 251, max: 350, text: "a grand piano 🎹", icon: "🎹" },
  { min: 351, max: 999, text: "a grizzly bear 🐻", icon: "🐻" }
]

const funweigth = computed(() => {
  const max = Math.round(oneRepMax.value)
  return funWeights.find(w => max >= w.min && max <= w.max) || { text: "unidentified beast 🐉", icon: "🐉" }
})

const benchcolor = computed(() => {
  const max = Math.round(oneRepMax.value)
  if (max < 40) return "#4caf50"
  if (max < 70) return "#8bc34a"
  if (max < 90) return "#cddc39"
  if (max < 110) return "#ffeb3b"
  if (max < 140) return "#ffc107"
  if (max < 180) return "#ff9800"
  if (max < 250) return "#ff5722"
  if (max < 350) return "#f44336" 
  return "#9c27b0"
})
</script>

<template>
  <div class="app-container">
    <Transition name="app-load" appear>
      <main>
      <div class="main-div">
        <header>
          <h1>Bench Press Max</h1>
        </header>
        
        <p>Enter your training data to find out your absolute potential.</p>
        
        <form @submit.prevent>
          <div class="row-inputs">
            <div class="input-group">
              <label for="weight">Weight (kg)</label>
              <input type="number" id="weight" v-model.number="weight" required />
            </div>
            <div class="input-group">
              <label for="reps">Reps</label>
              <input type="number" id="reps" v-model.number="reps" required />
            </div>
          </div>
        </form>
        
        <Transition name="slide-fade">
          <div v-if="oneRepMax > 0" class="result">
            <div class="progress-bar-container">
          <div class="progress-bar-fill" :style="{ width: `${Math.min((oneRepMax / 300) * 100, 100)}%`, backgroundColor: benchcolor }"></div></div>
            <span class="result-title">Your Estimated 1RM</span>
            <h2 :style="{ color: benchcolor, textShadow: `0 0 20px ${benchcolor}44` }">
              {{ Math.round(oneRepMax) }} <span class="unit">kg</span>
            </h2>
            <div class="funweigth">
              You can bench press {{ funweigth.text }}
            </div>
          </div>
        </Transition>
      </div>
    </main>
    </Transition>
  </div>
</template>

<style scoped>

:global(body) {
  margin: 0;
  padding: 0;
  background-color: #05070f;
}

.app-container {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  background-color: #05070f;
  background-image: radial-gradient(circle at center, v-bind(benchcolor) 0%, #05070f 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  width: 100vw;
  box-sizing: border-box;
  padding: 20px;
  transition: background 0.5s ease;
}


main {
  background: #0a192f;
  border: 1px solid #1e293b;
  border-radius: 24px;
  padding: 2.5rem;
  width: 100%;
  max-width: 440px; 
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.7);
  box-sizing: border-box;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

main:hover {
  transform: translateY(-5px) scale(1.01);
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.8);
}

.main-div {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

h1 {
  font-size: 1.6rem;
  font-weight: 800;
  letter-spacing: -0.02em;
  margin: 0 0 0.5rem 0;
  color: #fff;
  text-align: center;
}

p {
  color: #64748b;
  font-size: 0.85rem;
  line-height: 1.5;
  text-align: center;
  margin-bottom: 2rem;
}

form {
  width: 100%;
}

.row-inputs {
  display: flex;
  gap: 1.25rem;
  width: 100%;
  max-width: 320px; 
  margin: 0 auto; 
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex: 1;
}

label {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: #94a3b8;
  font-weight: 700;
  text-align: center;
}

input {
  background-color: #030712;
  border: 1px solid #1f2937;
  border-radius: 12px;
  color: #f3f4f6;
  padding: 0.8rem 0.5rem;
  font-size: 1.3rem; 
  font-weight: 700;
  text-align: center;
  transition: all 0.2s ease;
  width: 100%;
  box-sizing: border-box;
}

input:focus {
  outline: none;
  border-color: #38bdf8; 
  box-shadow: 0 0 0 4px rgba(56, 189, 248, 0.15);
  background-color: #090d16;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.result {
  margin-top: 2.5rem;
  padding-top: 2rem;
  border-top: 1px dashed #1e293b;
  text-align: center;
  width: 100%;
}

.result-title {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: #64748b;
  font-weight: 700;
}

.result h2 {
  font-size: 3.5rem;
  font-weight: 900;
  margin: 0.5rem 0;
  transition: all 0.4s ease;
}

.unit {
  font-size: 1.5rem;
  font-weight: 500;
  color: #64748b;
}

.funweigth {
  font-size: 0.95rem;
  color: #94a3b8;
  margin-top: 0.5rem;
  font-weight: 500;
}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}
.slide-fade-enter-from {
  transform: translateY(10px);
  opacity: 0;
}

.progress-bar-container {
  width: 100%;
  height: 4px;
  background: #030712;
  border-radius: 2px;
  margin: 1rem 0;
  overflow: hidden;
}
.progress-bar-fill {
  height: 100%;
  transition: width 0.8s cubic-bezier(0.16, 1, 0.3, 1), background-color 0.4s ease;
}
input:focus {
  outline: none;
  border-color: v-bind(benchcolor);
  box-shadow: 0 0 20px v-bind(benchcolor) + '22'; 
  background-color: #050914;
}

.app-load-enter-active {
  transition: opacity 1.6s ease-out, transform 1.6s cubic-bezier(0.16, 1, 0.3, 1), filter 1.6s ease-out;
}

.app-load-enter-from {
  opacity: 0;
  filter: blur(8px); 
  transform: translateY(40px) scale(0.96);
}
</style>