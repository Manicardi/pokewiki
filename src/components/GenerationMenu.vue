<template>
  <div class="button-container">
      <button id="left-btn" @click="changeGeneration('right')"><i class="fa fa-caret-left"></i></button>
      <div class="pagination" @click="changeGeneration(1)" :class=" generation == 1 ? 'current-page' : ''">1</div>
      <div class="pagination" @click="changeGeneration(2)" :class=" generation == 2 ? 'current-page' : ''">2</div>
      <div class="pagination" @click="changeGeneration(3)" :class=" generation == 3 ? 'current-page' : ''">3</div>
      <div class="pagination" @click="changeGeneration(4)" :class=" generation == 4 ? 'current-page' : ''">4</div>
      <div class="pagination" @click="changeGeneration(5)" :class=" generation == 5 ? 'current-page' : ''">5</div>
      <div class="pagination" @click="changeGeneration(6)" :class=" generation == 6 ? 'current-page' : ''">6</div>
      <button id="right-btn" @click="changeGeneration('left')"><i class="fa fa-caret-right"></i></button>
    </div>
</template>

<script>
export default {

  props: {
    generation: Number,
    changeGeneration: Function
  }

}
</script>

<style>

</style>