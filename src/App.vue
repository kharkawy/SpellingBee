<script setup lang="ts">
  import { ref } from 'vue';
  import Controls from './components/Controls.vue'
  import Status from './components/Status.vue'
  import type ControlLetter from './types/controlLetter';

  const completedWords = ref<string[]>([])
  const answers = ["Majorly", "Alarm", "Allay", "Alloy", "Ally", "Amoral", "Amorally", "Amyl", "Jolly", "Lama", "Llama", "Loam", "Loamy", "Loll", "Loom", "Lorry", "Loyal", "Loyally", "Mall", "Mammal", "Mayoral", "Molar", "Moll", "Molly", "Moola", "Moral", "Morally", "Oral", "Orally", "Rally", "Roll", "Royal", "Royally"].map(x => x.toLowerCase());
  const controlLetters: ControlLetter[] = [
    {
      value: "L",
      required: true
    },
    {
      value: "A",
    },
    {
      value: "J",
    },
    {
      value: "M",
    },
    {
      value: "O",
    },
    {
      value: "R",
    },
    {
      value: "Y",
    }
  ]

  let message = ref('');
  let showMessage = ref(false);

  function validateGuessWord(guessWord: string){

    guessWord = guessWord.toLowerCase();
    const requiredLetter: ControlLetter | undefined = controlLetters.find((letter) => letter.required)

    if(!requiredLetter) {
      return
    }
    
    
    if (guessWord.length < 4){
      message.value = "Too short"
    } else if (!guessWord.includes(requiredLetter.value.toLowerCase())) {
      console.log(requiredLetter.value)
      message.value = "Missing center letter"
    } else if (!answers.includes(guessWord)) {
      message.value = "Not in the word list"
    } else if (!completedWords.value.includes(guessWord)) {
      completedWords.value.push(guessWord);
      message.value = "Nice!"
    }

    showMessage.value = true

    setTimeout(() => {
      showMessage.value = false
      setTimeout(() => {
        message.value = ""
      }, 300)
    }, 3000)
  }

</script>

<template>
  <header>
    <span :class="[{ active: showMessage}, 'message']">{{ message }}</span>
    <div class="wrapper">
      <Controls :controlLetters="controlLetters" @submit="validateGuessWord"/>
      <Status :completedWords="completedWords"/>
    </div>
  </header>

</template>
