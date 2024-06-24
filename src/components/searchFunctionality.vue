<template>
  <div class="search-container text-align-center">
    <form action="" class="form d-flex justify-content-center">
      <input
        type="text"
        name="q"
        class="search-input my-2"
        placeholder="Search for advice"
        v-model="searchWord"
      />
      <button type="submit" @click.prevent="search(searchWord)" class="search-button btn m" :value="buttonValue">
        {{ buttonValue }}
      </button>
    </form>
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
import { defineEmits } from 'vue';

const emit = defineEmits(['searchEmit'])

const searchWord = ref([]);
const search =(word)=>{
   
    emit('searchEmit',word);
    searchWord.value=''
}
const buttonValue = computed(() => {
  return searchWord.value.length === 0 ? "Random Search" : "Search";
});
</script>
<style scoped>
.search-container {
  top: 20%;
  width: 100%;
  text-align: center;
  border: 0;
  list-style-type: none;
}
.search-input {
  width: 50%;
  height: 40px;

  width: 75%;
  height: 40px;
  background-color: beige;
  border: 0;
}
.search-button {
  line-height: 1rem;
  padding: 0 20px;
  margin-top: 7px;
  height: 40px;
  background-color: rgb(186, 186, 44);
  font-size: 0.9rem;
  font-weight: 500;
}
</style>
