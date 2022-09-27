<script setup lang="ts">
    import { ref } from 'vue';
    import type ControlLetter from '../types/controlLetter'

  const props = defineProps<{controlLetters: ControlLetter[]}>();

  const emit = defineEmits(['submit'])

  const inputValue = ref("");
  const controlLetters = ref(props.controlLetters)

  function clearInput(){
    inputValue.value = "";
  }

  function addLetterToInput(letter: string){
    inputValue.value = [...inputValue.value, letter].join('')
  }

  function deleteLetterFromInput(){
    const inputCharArray = [...inputValue.value];
    inputCharArray.pop();
    inputValue.value = inputCharArray.join('');

  }

  function checkIfLetter(event: KeyboardEvent){
    if(/^[A-Za-z]+$/.test(event.key)) return true;
    else event.preventDefault();
  }

  function shuffle(array: ControlLetter[]) {
    const requiredLetters = array.filter(letter => letter.required)
    const nonRequiredLetters = array.filter(letter => !letter.required)
    let length = nonRequiredLetters.length;
    
    // While there remain elements to shuffle…
    while (length) {

      // Pick a remaining element…
      const i = Math.floor(Math.random() * length--);

      // And swap it with the current element.
      const t = nonRequiredLetters[length];
      nonRequiredLetters[length] = nonRequiredLetters[i];
      nonRequiredLetters[i] = t;
    }

    return [...requiredLetters, ...nonRequiredLetters];
  }

  function rotateLetters(){
    controlLetters.value = shuffle(controlLetters.value)
  }

</script>

<template>
  <div class="controls-container">
    <div class="input-container">
      <input v-model="inputValue" @keyup.enter="$emit('submit', inputValue); clearInput();" @keypress="checkIfLetter($event)" type="text" placeholder="Type or click"/>
    </div>
    <div class="honeycomb-container">
      <div class="honeycomb">
        <svg v-for="controlLetter in controlLetters" v-on:click="addLetterToInput(controlLetter.value)" :class="[controlLetter.required ? 'center' : 'outer', 'honeycomb-cell']" viewBox="0 0 120 103.92304845413263">
          <polygon
            class="cell-fill"
            points="0,51.96152422706631 30,0 90,0 120,51.96152422706631 90,103.92304845413263 30,103.92304845413263"
            stroke="white"
            stroke-width="7.5"
          ></polygon>
          <text class="cell-letter" x="50%" y="50%" dy="0.35em">{{ controlLetter.value }}</text>
        </svg>
      </div>
    </div>
    <div class="buttons-container">
      <button type="submit" v-on:click="$emit('submit', inputValue); clearInput()">Enter</button>
      <button v-on:click="deleteLetterFromInput()">Delete</button>
      <button v-on:click="rotateLetters()">Rotate</button>
    </div>
  </div>
</template>

<style>
.honeycomb-container {
  max-width: 400px;
  width: 90%;
  margin: 25px auto;
  -webkit-tap-highlight-color: transparent;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.honeycomb {
  position: relative;
  width: 100%;
  padding-bottom: 103.92305%;
}
.honeycomb .cell-letter {
  font-weight: 700;
  font-size: 1.875em;
  text-anchor: middle;
  text-transform: uppercase;
  pointer-events: none;
}
.honeycomb-cell {
  position: absolute;
  top: calc(100% / 3);
  left: 30%;
  width: 40%;
  height: calc(100% / 3);
}
.honeycomb-cell .cell-fill {
  cursor: pointer;
  fill: #e6e6e6;
  transition: all 100ms;
}
.honeycomb-cell.center .cell-fill {
  cursor: pointer;
  fill: #f7da21;
  transition: all 100ms;
}
.honeycomb-cell.center {
  transform: translate(0, 0);
}
.honeycomb-cell:nth-child(2) {
  transform: translate(-75%, -50%);
}
.honeycomb-cell:nth-child(3) {
  transform: translate(0, -100%);
}
.honeycomb-cell:nth-child(4) {
  transform: translate(75%, -50%);
}
.honeycomb-cell:nth-child(5) {
  transform: translate(75%, 50%);
}
.honeycomb-cell:nth-child(6) {
  transform: translate(0, 100%);
}
.honeycomb-cell:nth-child(7) {
  transform: translate(-75%, 50%);
}

@media (max-width: 375.98px) {
  .honeycomb-container {
    width: 70%;
    margin: 4vh auto;
  }
  .honeycomb .cell-letter {
    font-size: 2.5em;
  }
}
</style>
