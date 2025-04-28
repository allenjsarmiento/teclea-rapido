<template>

  <div class="container">
    <h1>Batalla de Teclas</h1>
  
    <div class="text-container">
      <div>
        <p class="text-score">{{numberKeyPressed}}/{{ text.length }}</p>
        <span 
          v-for="(char,index) in text" 
          :key="index"
          :class="{
            'text-success': index < currentIndex,
            'text-gray': index >= currentIndex,
            'inline-block animate__animated animate__shakeX': index === shakeIndex && char !== ' ',
          }"
        >
          {{ char }}
        </span>
        <p class="text-win" v-if="winGame"><cite>{{ author }}</cite></p>
      </div>
      
    </div>

   

  </div>
  
  
</template>

<script lang='ts' setup>
import { onMounted,computed,ref } from 'vue'
import frases from './assets/frases.json'

const text = ref('')
const author = ref()
const currentIndex = ref(0)
const numberKeyPressed = ref(0)
const shakeIndex = ref<number|null>(null)

const handleKeydown = (event: KeyboardEvent) => {
  const keyPressed = event.key
  const keyExpected = text.value[currentIndex.value]

  // Si NO es una tecla normal (ej: Shift, Alt, Ctrl...), salimos al toque
  if (keyPressed.length !== 1) {
    return
  }

  numberKeyPressed.value++ // ✅ Ahora sí, contamos la tecla válida

  if (keyPressed === keyExpected) {
    currentIndex.value++

    if(currentIndex.value === text.value.length){
      alert('Ganaste!')
    }

  }else{
    shakeIndex.value = currentIndex.value
    setTimeout(() => {
      shakeIndex.value = null
    }, 500);
  }

}

const getFrase = () => {
  const randomIndex = Math.floor(Math.random() * frases.length)
  text.value = frases[randomIndex].text
  author.value = frases[randomIndex].author
}

const startGame = () => {
  getFrase()
  currentIndex.value = 0
  
}

const winGame = computed(() => {
  return currentIndex.value === text.value.length
})

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
  startGame()
})

</script>

<style scoped>

</style>