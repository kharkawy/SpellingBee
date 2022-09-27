<script lang="ts">
    export default {
      props: {
        completedWords: Array
      }
    }
</script>

<template>
  <div class="status-container">
    <ul>
      <li v-for="word in completedWords">
        {{ word }}
      </li>
    </ul>
  </div>
</template>