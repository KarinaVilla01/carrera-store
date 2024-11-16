<script setup>
const props = defineProps(['randomRef'])
const emit = defineEmits(['moveForward'])
async function fetchData(){
  //TODO: Call the right api waiting for a success response to display the message
  const value=await (await fetch(`https://api.carreracarwash.com/Prosepago/purchase-order/${props.randomRef}/completed`, {mode: 'cors'})).json()
  if(value){emit('moveForward')}
}

setInterval(fetchData, 5000)
</script>

<template>
  <div class="loader-center">
    <div class="loader-wrapper">
      <span class="loader"></span>
    </div>

    <div>
      <span>Referencia: {{props.randomRef}}</span>
    </div>
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
.loader-center{
  display: grid;
  justify-content: center;
  height: 100%;
}

.loader-wrapper{
  display: flex;
  justify-content: center;
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