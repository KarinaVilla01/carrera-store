<script setup>
  function openCart(){

  }
</script>

<template>
  <main>
    <div class="cart-price">
      <h6>Shopping Cart</h6>
      <ul class="listCart">
      </ul>
      <div class="checkOut">
        <div class="total">$0</div>
        <div class="closeShopping">Close</div>
      </div>
    </div>
    <div id="button-create">
      <button id="buy" @click="openCart">Comprar</button>
    </div>
  </main>
</template>

<style scoped>

</style>
