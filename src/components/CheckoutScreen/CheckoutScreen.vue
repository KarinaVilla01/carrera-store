<script setup>

</script>

<template>
  <form action="https://ecom.prosepago.com/tvirtual.aspx" method="post" target="_blank">
    <input name="t" id="t" type="hidden" value="35145"/>
    <input name="medio_pago" id="medio_pago" type="hidden" value="0"/>
    <input name="msi_val" id="msi_val" type="hidden" value="0"/>
    Nombre: <input name="nom" id="nom" type="text" value="Eduardo"/>
    <input name="con" id="con" type="hidden" value="Testsssss"/>
    <input name="ref" id="ref" type="hidden" value="69420"/>
    <input name="imp" id="imp" type="hidden" value="0.01"/>
    Correo:<input name="ema" id="ema" type="text" value="eduardolaborin@gmail.com"/>
    <input name="urlok" id="urlOk" type="hidden" value="https://google.com"/>
    <input name="urlko" id="urlKo" type="hidden" value="https://youtube.com"/>
    <input name="autoBack" id="autoBack" type="hidden" value="1"/>
    <input type="submit" value="Submit" onclick="alert('hello')">
  </form>
</template>

<style scoped>

</style>