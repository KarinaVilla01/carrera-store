<script setup>
import { defineProps, defineEmits, ref } from 'vue'
import img1 from '../imgs/1.png'
import img2 from '../imgs/2.png'
import img3 from '../imgs/3.png'
import img4 from '../imgs/4.png'

const emit = defineEmits(['update:selected']);

const options = ['Paquete', 'Membresía'];
const selectedOption = ref(null);
const selectedPackageId = ref(null);
const packages = ref([
  {
    _id: "612abcd1c4ce4c141237a356",
    image: img1,
    name: "Supremo",
    pricePackage: 400,
    primeMembership: 1000,
    isSelected: false,
  },
  {
    _id: "612f1c4f30b90803837e7969",
    image: img4,
    name: "Ultra",
    pricePackage: 300,
    primeMembership: 800,
    isSelected: false,
  },
  {
    _id: "612f067387e473107fda56b0",
    image: img3,
    name: "Basico",
    pricePackage: 200,
    primeMembership: 650,
    isSelected: false,
  },
  {
    _id: "612f057787e473107fda56aa",
    image: img2,
    name: "Express",
    pricePackage: 100,
    primeMembership: 500,
    isSelected: false,
  },
]);

function emitSelection() {
  emit("update:selected", selectedOption.value)
}

const props = defineProps({
  options: {
    type: Array,
    required: true
  }
})

const selectOption = (option) => {
  selectedOption.value = option;
  emitSelection();
};

const selectPackage = (pack) => {
  selectedPackageId.value = pack._id;

  packages.value.forEach(p => {
    p.isSelected = p._id === selectedPackageId.value;
  });
  emitSelection();
};

</script>

<template>
  <h5>Selecciona tu compra:</h5>
  <div class="selection">
    <div v-for="option in options" :key="option" :class="{ 'selection-btn': true, 'selected': selectedOption === option }"
      @click="selectOption(option)">
      {{ option }}
    </div>
  </div>
  <div v-if="selectedOption">
    <h5>Elije tu {{ selectedOption }}:</h5>
    <ul class="listCartInto">
      <li v-for="pack in packages" :key="pack._id">
        <a :href="'#/package/' + pack._id" @click="selectPackage(pack)" :class="{ 'selectedOption': pack.isSelected }">
          <div><img :src="pack.image" alt="Package Image"></div>
          <div>{{ pack.name }}</div>
          <div v-if="selectedOption === 'Paquete'">${{ pack.pricePackage }}</div>
          <div v-if="selectedOption === 'Membresía'">${{ pack.primeMembership }}</div>
        </a>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>