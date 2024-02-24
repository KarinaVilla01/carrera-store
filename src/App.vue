<script>
import {ref, toRefs} from "vue"
import packagesData from "@/constants/packagesData.js";
import OrderTypeSelector from "@/components/SelectionScreen/OrderTypeSelector.vue";
import CheckoutCompleteScreen from "@/components/CheckoutCompleteScreen/CheckoutCompleteScreen.vue";
import packageTypeEnum from "@/constants/packageTypeEnum.js";

export default {
  computed: {
    packageTypeEnum() {
      return packageTypeEnum
    }
  },
  components: {CheckoutCompleteScreen, OrderTypeSelector},
  setup() {
    const packages = ref(packagesData);
    const showCart = ref(false);

    const messageTop = ref(false);
    const allwaysSelect = ref("option1");
    const allwaysSelect2 = ref("option2");
    const paymentCorrect = ref(false);
    const activatePopup = ref(false);
    const selectedOrderType = ref(null);
    const selectedPackageId = ref(null);
    const selectedPackage = ref(null)
    const confirmShopping = ref(false);
    const confirmPayment = ref(false);
    const confirmCartPayment = ref(false);
    const currentCardBackground = ref(Math.floor(Math.random() * 25 + 1));
    const cardName = ref("");
    const cardNumber = ref("");
    const cardMonth = ref("");
    const cardYear = ref("");
    const cardCvv = ref("");
    const minCardYear = ref(new Date().getFullYear());
    const amexCardMask = ref("#### ###### #####");
    const otherCardMask = ref("#### #### #### ####");
    const cardNumberTemp = ref("");
    const isCardFlipped = ref(false);
    const focusElementStyle = ref(null);
    const isInputFocused = ref(false);

    const toggleCart = () => {
      showCart.value = !showCart.value;
    };

    const autorizePayment = () => {
      paymentCorrect.value = !paymentCorrect.value;
    }

    const changeMesage = () => {
      messageTop.value = !messageTop.value;
    }




    const selectPackage = (pack) => {
      selectedPackage.value = pack;
      selectedPackageId.value = pack._id;

      packages.value.forEach(p => {
        p.isSelected = p._id === selectedPackageId.value;
      });

    };

    const activatedPopup = () => {
      activatePopup.value = !activatePopup.value;
      confirmPayment.value = !confirmPayment.value;
    }

    const confirmShoppingCart = () => {
      confirmShopping.value = !confirmShopping.value;
    }

    const confirmPaymentMethod = () => {
      confirmCartPayment.value = !confirmCartPayment.value;
      messageTop.value = !messageTop.value;
      confirmPayment.value = !confirmPayment.value;
    }

    const calculateSubtotal = () => {
      if (selectedOrderType.value === packageTypeEnum.paquete && selectedPackageId.value) {
        const selectedPackage = packages.value.find(pack => pack._id === selectedPackageId.value);
        if (selectedPackage) {
          return selectedPackage.pricePackage
        }
      } else if (selectedOrderType.value === packageTypeEnum.membresia && selectedPackageId.value) {
        const selectedPackage = packages.value.find(pack => pack._id === selectedPackageId.value);
        if (selectedPackage) {
          return selectedPackage.priceMembership
        }
      } else {
        return 0;
      }
    };

    const mounted = () => {
      cardNumberTemp.value = otherCardMask.value;
      document.getElementById("cardNumber").focus();
    };

    const getCardType = () => {
      let number = cardNumber.value;
      let re = new RegExp("^4");
      if (number.match(re) != null) return "visa";

      re = new RegExp("^(34|37)");
      if (number.match(re) != null) return "amex";

      re = new RegExp("^5[1-5]");
      if (number.match(re) != null) return "mastercard";

      re = new RegExp("^6011");
      if (number.match(re) != null) return "discover";

      re = new RegExp('^9792');
      if (number.match(re) != null) return 'troy';

      return "visa"; // default type
    };

    const generateCardNumberMask = () => {
      return getCardType() === "amex" ? amexCardMask.value : otherCardMask.value;
    };

    const minCardMonth = () => {
      if (cardYear.value === minCardYear.value) return new Date().getMonth() + 1;
      return 1;
    };

    const flipCard = (status) => {
      isCardFlipped.value = status;
    };

    const focusInput = (e) => {
      isInputFocused.value = true;
      let targetRef = e.target.dataset.ref;
      let target = toRefs(this.$refs)[targetRef];
      focusElementStyle.value = {
        width: `${target.offsetWidth}px`,
        height: `${target.offsetHeight}px`,
        transform: `translateX(${target.offsetLeft}px) translateY(${target.offsetTop}px)`
      };
    };

    const blurInput = () => {
      setTimeout(() => {
        if (!isInputFocused.value) {
          focusElementStyle.value = null;
        }
      }, 300);
      isInputFocused.value = false;
    };

    const optionImageSrc = (value) => {
      return "src/_assets/packageIco.png"
    };

    return {
      packages,
      showCart,
      optionImageSrc,
      selectedOrderType,
      selectedPackageId,
      confirmShopping,
      confirmPayment,
      confirmCartPayment,
      currentCardBackground,
      cardName,
      cardNumber,
      cardMonth,
      cardYear,
      cardCvv,
      minCardYear,
      amexCardMask,
      otherCardMask,
      cardNumberTemp,
      isCardFlipped,
      focusElementStyle,
      isInputFocused,
      messageTop,
      allwaysSelect,
      allwaysSelect2,
      selectedPackage,
      activatePopup,
      paymentCorrect,
      autorizePayment,
      activatedPopup,
      changeMesage,
      mounted,
      getCardType,
      generateCardNumberMask,
      minCardMonth,
      flipCard,
      focusInput,
      blurInput,
      toggleCart,
      selectPackage,
      calculateSubtotal,
      confirmShoppingCart,
      confirmPaymentMethod
    };
  },
};
</script>

<template>
  <main>
    <div :class="{ 'cart-price': true, 'active': showCart }">
      <div v-if="paymentCorrect === false" class="cart-header">
        <i v-if="activatePopup === false" class="fa-solid fa-bag-shopping"></i>
        <i v-if="activatePopup === true" class="fa-solid fa-circle-exclamation"></i>
        <h1 v-if="messageTop === false && activatePopup === false">CARRITO DE COMPRA</h1>
        <h1 v-if="messageTop === true && activatePopup === false">RESUMEN</h1>
        <h1 v-if="activatePopup === true">ATENCIÓN</h1>
        <span id="closeBtn" class="close-btn" @click="toggleCart">&times;</span>
      </div>
      <div class="listCart" v-if="confirmShopping === false && confirmCartPayment === false">
        <OrderTypeSelector v-model="selectedOrderType"/>
        <p v-if="selectedOrderType === packageTypeEnum.membresia" class="parra">Tu <span>membresía</span> se renovará mensualmente de forma
          atomática.</p>
        <div v-if="selectedOrderType">
          <h4>SELECCIONA TU SERVICIO PREFERIDO</h4>
          <ul class="listCartInto">
            <li v-for="pack in packages" :key="pack._id" :style="{ backgroundImage: 'url(' + pack.image + ')' }"
              @click="selectPackage(pack)" :class="{ 'selectedOrderType': pack.isSelected }">
              <a :href="'#/package/' + pack._id">
                <div class="packLetters">{{ pack.name }}</div>
                <div v-if="selectedOrderType === packageTypeEnum.paquete" class="packLetters">${{ pack.pricePackage }} <span>.00</span>
                </div>
                <div v-if="selectedOrderType === packageTypeEnum.membresia" class="packLetters">${{ pack.priceMembership }}<span>.00</span>
                </div>
              </a>
            </li>
          </ul>
        </div>
      </div>
      <div v-if="confirmShopping === true && confirmCartPayment === false && activatePopup === false">
      </div>
      <div v-if="activatePopup === true && paymentCorrect === false" class="popUp">
        <div>
          <i class="fa-solid fa-circle-exclamation"></i>
          <!-- <img src="./_assets/Confirmation Ico.png" alt=""> -->
          <p>Al aceptar, estás de acuerdo con un <span>cargo recurrente mensual</span>
            y deberás firmar en ventanilla durante tu primera visita.</p>
        </div>
        <button @click="autorizePayment">ACEPTO</button>
      </div>
      <div v-if="paymentCorrect === true" class="paymentPage">
        <CheckoutCompleteScreen :selected-order-type="selectedOrderType" :selected-package="selectedPackage"/>
      </div>
      <div v-if="paymentCorrect === false" class="checkOut">
        <div class="subtotal">
          <p>SUBTOTAL:</p>
          <p>$<span id="subtotal">{{ calculateSubtotal() }}.00</span></p>
        </div>
        <button v-if="calculateSubtotal() !== 0 && confirmShopping === false"
          @click="confirmShoppingCart()">Confirmar pedido</button>
        <button v-if="confirmShopping === true && confirmCartPayment === false && activatePopup === false"
          @click="confirmPaymentMethod()">REALIZAR EL PAGO</button>
        <button v-if="confirmPayment === true && activatePopup === false && selectedOrderType === packageTypeEnum.membresia"
          @click="activatedPopup">CONFIRMAR PAGO</button>
        <button v-if="confirmPayment === true && selectedOrderType === packageTypeEnum.paquete" @click="autorizePayment">CONFIRMAR
          PAGO</button>
        <div v-if="confirmPayment === true && selectedOrderType === packageTypeEnum.membresia" class="terminos">
          <p>Al proceder, aceptas los Términos y Condiciones y <br> reconoces que tu membresía
            <span>se renovará <br>automáticamente con cargo mensual.</span>
          </p>
        </div>
      </div>
      {{selectedOrderType}}
      {{paymentCorrect}}

    </div>
    <div id="button-create">
      <button id="buy" @click="toggleCart">Comprar</button>
    </div>
  </main>
</template>


<style scoped>
button:hover {
  transition: 0.3s;
  opacity: 0.8;
}
</style>
