<template>
  <h1 class="bg-red px-8 py-2 sign text-white mx-auto rounded-full inline-block">
      <Letter v-for="(letter, pos) in textProp.split('')"
        :key="`${letter}-${pos}`"
        :letter="letter"
        :pos="pos"/>
  </h1>
</template>

<script>
import Letter from './CrookedLetter';

export default {
  name: 'Sign',
  props: {
    textProp: String,
  },
  components: {
    Letter,
  },
};
</script>

<style>
.sign {
    font-family: 'Bungee Outline', cursive;
    text-shadow: white 0 0 10px, white 0 0 20px, white 0 0 30px;
}

@keyframes flicker {
    0% {
        color: white;
        text-shadow: white 0 0 10px, white 0 0 20px, white 0 0 30px;
    }
    1% {
        color: black;
        text-shadow: none;
    }
    39% {
        color: black;
        text-shadow: none;
    }
    40% {
        color: white;
        text-shadow: white 0 0 10px, white 0 0 20px, white 0 0 30px;
    }
    47% {
        color: white;
        text-shadow: white 0 0 10px, white 0 0 20px, white 0 0 30px;
    }
    48% {
        color: black;
        text-shadow: none;
    }
    50% {
        color: white;
        text-shadow: white 0 0 10px, white 0 0 20px, white 0 0 30px;
    }
    51% {
        color: black;
        text-shadow: none;
    }
    53% {
        color: white;
        text-shadow: white 0 0 10px, white 0 0 20px, white 0 0 30px;
    }
}
</style>
