<template>
  <div class="parent">
    <div
      class="container wraper d-flex flex-column align-items-center jusitify-content-center"
    >
      <h2>Begin your day with advice</h2>
      <searchFunctionality @searchEmit="search" />
      <loadingComponent v-if="isLoading" />
      <div class="message-container container" v-else>
        <p v-if="erroeMessage">{{ erroeMessage }}</p>
        <p class="my-3" v-for="advice in data" :key="advice.id" v-else>
          {{ advice.advice }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onBeforeMount, ref } from "vue";
import searchFunctionality from "./searchFunctionality.vue";
import loadingComponent from "./UI/loadingComponent.vue";
const data = ref();
const erroeMessage = ref();
const isLoading = ref(false);
const search = async (word) => {
  let url = null;

  const parsed = parseFloat(word);

  if (!isNaN(parsed) && isFinite(parsed)) {
    url = `https://api.adviceslip.com/advice/${word}`;
  } else if (typeof word === "string" && word.trim() != "") {
    url = `https://api.adviceslip.com/advice/search/${word.trim()}`;
  } else {
    url = "https://api.adviceslip.com/advice";
  }
  adviceRequest(url);
};
const adviceRequest = async (url) => {
  isLoading.value = true;
  erroeMessage.value = "";
  try {
    const response = await fetch(url);
    if (!response.ok) {
      erroeMessage.value =
        "Opppps,something went wrong while getting your advice";
      throw new Error(`Server Error: ${response.status}`);
    }
    const responseData = await response.json();
    if (responseData.message) {
      erroeMessage.value = "No result match your search word";
    }
    data.value = responseData.slip ? responseData : responseData.slips;
  } catch (Err) {
    erroeMessage.value =
      "Timeout!! please check your internet connection and try again";
    throw Err;
  } finally {
    isLoading.value = false;
  }
};
onBeforeMount(()=>{
  search();
})
</script>

<style scoped>
.parent {
  position: relative;
 background-size: fill;
 overflow: auto;
  background-image: url('../assets/Img/beautiful-beach-sea-with-palm-tree.jpg');
  background-position: center;
  min-height: 100vh; 
  height: auto; 
}
.wraper {
  margin-top: 50px;
  position: relative;
  overflow: hidden;
  width: 100%;
}

.message-container {
  background-color: chocolate;
  padding: 20px 20px;
  height: fit-content;
  width: fit-content;
  padding: auto;
  max-width: 700px;
  line-height: 1;
  border-radius: 25px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}
.message-container p {
  margin: 10px;
  font-size: 1.3rem;
  margin: 2px 0;
}
.message-container {
  animation: bounce 2s ease-in-out 1;
}

@keyframes bounce {
  0% { transform: translateY(0); }
  50% { transform: translateY(20px); }
  100% { transform: translateY(0); }
}
@media (max-width: 1024px) {
  .message-container p {
    font-size: 1.1rem;
  }
  .wraper {
    margin-top:25px;
  }
}
</style>
