<script setup>
function authorizePaument(){
  console.log("next")
}
</script>

<template>
  <div class="popUp">
    <div>
      <i class="fa-solid fa-circle-exclamation"></i>
      <!-- <img src="./_assets/Confirmation Ico.png" alt=""> -->
      <p>Al aceptar, estás de acuerdo con un <span>cargo recurrente mensual</span>
        y deberás firmar en ventanilla durante tu primera visita.</p>
    </div>
    <button @click="$emit('moveForward')">ACEPTO</button>
  </div>
</template>

<style scoped>

</style>