<script setup>
async function fetchData(){
  //TODO: Call the right api waiting for a success response to display the message
  // const value=await (await fetch("https://randomuser.me/api", {mode: 'cors'})).json()
  // console.log(value)
}

setInterval(fetchData, 5000)
</script>

<template>
  <div class="loader-center">
    <span class="loader"></span>
  </div>
</template>

<style scoped>
.loader {
  width: 48px;
  height: 48px;
  border: 5px solid #FFF;
  border-bottom-color: transparent;
  border-radius: 50%;
  display: inline-block;
  box-sizing: border-box;
  animation: rotation 1s linear infinite;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>