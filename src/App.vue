<template>
  <div class="p-6">
    <!-- Rada ikon planet -->
    <div class="flex space-x-4">
      <button 
        v-for="planet in planets" 
        :key="planet" 
        @click="selectPlanet(planet)"
        :class="['p-2 rounded-full cursor-pointer transition-colors', currentPlanet === planet ? 'bg-blue-500 text-white' : 'bg-gray-200 text-black']"
      >
        {{ planet }}
      </button>
    </div>
    
    <!-- Rada znamení -->
    <div class="flex space-x-4 mt-4">
      <button 
        v-for="sign in signs" 
        :key="sign" 
        @click="selectSign(sign)"
        :class="['p-2 rounded-full cursor-pointer transition-colors', currentSign === sign ? 'bg-green-500 text-white' : 'bg-gray-200 text-black']"
      >
        {{ sign }}
      </button>
    </div>
    
    <!-- Textový rámeček pro interpretace -->
    <div class="mt-6">
      <div class="flex items-center justify-between">
        <button @click="prevInterpretation" class="p-2 bg-gray-300 rounded-full">←</button>
        <textarea 
          class="w-full h-32 p-4 border rounded-md"
          readonly>
          {{ currentInterpretation }}
        </textarea>
        <button @click="nextInterpretation" class="p-2 bg-gray-300 rounded-full">→</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { constelation } from './constelation.ts';

const signs = ['aries', 'taurus', 'gemini', 'cancer', 'leo', 'virgo', 'libra', 'scorpio', 'sagittarius', 'capricorn', 'aquarius', 'pisces'];
const planets = ['sun', 'moon', 'mars', 'venus', 'jupiter', 'saturn', 'uranus', 'neptune', 'pluto'];
// const constelation = {
//   suninaries: ['aktivni jedinec', 'pracovita'],
//   sunintaurius: ['usedla povaha'],
//   // ostatní konstelace...
// };

// Výchozí stav - přednastavení Sun/Aries
const currentPlanet = ref(null);
const currentSign = ref(null);
const interpretationIndex = ref(0);

// Pamatování nastavení pro každou planetu
const selections = ref(
  planets.reduce((acc, planet) => {
    acc[planet] = { sign: 'aries', interpretationIndex: 0 }; // Výchozí znamení pro každou planetu je Aries
    return acc;
  }, {})
);

// Výběr planety
const selectPlanet = (planet) => {
  currentPlanet.value = planet;
  currentSign.value = selections.value[planet].sign;
  interpretationIndex.value = selections.value[planet].interpretationIndex;
};

// Výběr znamení
const selectSign = (sign) => {
  currentSign.value = sign;
  interpretationIndex.value = 0;
  selections.value[currentPlanet.value].sign = sign;
};

// Výpočet aktuální interpretace
const currentInterpretation = computed(() => {
  const key = `${currentPlanet.value}in${currentSign.value}`;
  return constelation[key]?.[interpretationIndex.value] || 'Žádná interpretace';
});

// Navigace mezi interpretacemi
const nextInterpretation = () => {
  const key = `${currentPlanet.value}in${currentSign.value}`;
  if (constelation[key] && interpretationIndex.value < constelation[key].length - 1) {
    interpretationIndex.value++;
    selections.value[currentPlanet.value].interpretationIndex = interpretationIndex.value;
  }
};

const prevInterpretation = () => {
  if (interpretationIndex.value > 0) {
    interpretationIndex.value--;
    selections.value[currentPlanet.value].interpretationIndex = interpretationIndex.value;
  }
};

// Nastavení výchozích hodnot (Sun/Aries)
onMounted(() => {
  currentPlanet.value = 'sun';
  currentSign.value = 'aries';
});

</script>

<style scoped>
/* TailwindCSS přidá stylování automaticky */
</style>
