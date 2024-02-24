<script>
import { ref } from "vue"

import img1 from '../src/imgs/1.png'
import img2 from '../src/imgs/2.png'
import img3 from '../src/imgs/3.png'
import img4 from '../src/imgs/4.png'
export default {
  setup() {
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
    const showCart = ref(false);
    const options = ['Paquete', 'Membresía'];
    const selectedOption = ref(null);
    const selectedPackageId = ref(null);
    const confirmateShooping = ref(false);
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

    const selectOption = (option) => {
      selectedOption.value = option;
    };

    const selectPackage = (pack) => {
      selectedPackageId.value = pack._id;

      packages.value.forEach(p => {
        p.isSelected = p._id === selectedPackageId.value;
      });
    };

    const confirmateShoppingCart = () => {
      confirmateShooping.value = !confirmateShooping.value;

    }

    const calculateSubtotal = () => {
      if (selectedOption.value === 'Paquete' && selectedPackageId.value) {
        const selectedPackage = packages.value.find(pack => pack._id === selectedPackageId.value);
        if (selectedPackage) {
          return selectedPackage.pricePackage
        }
      } else if (selectedOption.value === 'Membresía' && selectedPackageId.value) {
        const selectedPackage = packages.value.find(pack => pack._id === selectedPackageId.value);
        if (selectedPackage) {
          return selectedPackage.primeMembership
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

    return {
      packages,
      showCart,
      options,
      selectedOption,
      selectedPackageId,
      confirmateShooping,
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
      mounted,
      getCardType,
      generateCardNumberMask,
      minCardMonth,
      flipCard,
      focusInput,
      blurInput,
      toggleCart,
      selectOption,
      selectPackage,
      calculateSubtotal,
      confirmateShoppingCart,
    };
  },
};
</script>

<template>
  <main>
    <div :class="{ 'cart-price': true, 'active': showCart }">
      <div class="cart-header">
        <img alt="shop" src="@/imgs/shopping.svg">
        <h1>Tu Carrito</h1>
        <span id="closeBtn" class="close-btn" @click="toggleCart">&times;</span>
      </div>
      <div class="listCart" v-if="confirmateShooping === false">
        <h5>Selecciona tu compra:</h5>
        <div class="selection">
          <div v-for="option in options" :key="option"
            :class="{ 'selection-btn': true, 'selected': selectedOption === option }" @click="selectOption(option)">
            {{ option }}
          </div>
        </div>
        <div v-if="selectedOption">
          <h5>Elije tu {{ selectedOption }}:</h5>
          <ul class="listCartInto">
            <li v-for="pack in packages" :key="pack._id">
              <a :href="'#/package/' + pack._id" @click="selectPackage(pack)"
                :class="{ 'selectedOption': pack.isSelected }">
                <div><img :src="pack.image" alt="Package Image"></div>
                <div>{{ pack.name }}</div>
                <div v-if="selectedOption === 'Paquete'">${{ pack.pricePackage }}</div>
                <div v-if="selectedOption === 'Membresía'">${{ pack.primeMembership }}</div>
              </a>
            </li>
          </ul>
        </div>
      </div>
      <div v-if="confirmateShooping === true">
        <div class="card-form">
          <div class="card-list">
            <div class="card-item" v-bind:class="{ '-active': isCardFlipped }">
              <div class="card-item__side -front">
                <div class="card-item__focus" v-bind:class="{ '-active': focusElementStyle }"
                  v-bind:style="focusElementStyle" ref="focusElement"></div>
                <div class="card-item__cover">
                  <img
                    v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + currentCardBackground + '.jpeg'"
                    class="card-item__bg">
                </div>

                <div class="card-item__wrapper">
                  <div class="card-item__top">
                    <img
                      src="https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/chip.png"
                      class="card-item__chip">
                    <div class="card-item__type">
                      <transition name="slide-fade-up">
                        <img
                          v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + getCardType + '.png'"
                          v-if="getCardType" v-bind:key="getCardType" alt="" class="card-item__typeImg">
                      </transition>
                    </div>
                  </div>
                  <label for="cardNumber" class="card-item__number">
                    <template v-if="getCardType === 'amex'">
                      <span v-for="(n, $index) in amexCardMask" :key="$index">
                        <transition name="slide-fade-up">
                          <div class="card-item__numberItem"
                            v-if="$index > 4 && $index < 14 && cardNumber.length > $index && n.trim() !== ''">*</div>
                          <div class="card-item__numberItem" :class="{ '-active': n.trim() === '' }" :key="$index"
                            v-else-if="cardNumber.length > $index">
                            {{ cardNumber[$index] }}
                          </div>
                          <div class="card-item__numberItem" :class="{ '-active': n.trim() === '' }" v-else
                            :key="$index + 1">{{ n }}</div>
                        </transition>
                      </span>
                    </template>

                    <template v-else>
                      <span v-for="(n, $index) in otherCardMask" :key="$index">
                        <transition name="slide-fade-up">
                          <div class="card-item__numberItem"
                            v-if="$index > 4 && $index < 15 && cardNumber.length > $index && n.trim() !== ''">*</div>
                          <div class="card-item__numberItem" :class="{ '-active': n.trim() === '' }" :key="$index"
                            v-else-if="cardNumber.length > $index">
                            {{ cardNumber[$index] }}
                          </div>
                          <div class="card-item__numberItem" :class="{ '-active': n.trim() === '' }" v-else
                            :key="$index + 1">{{ n }}</div>
                        </transition>
                      </span>
                    </template>
                  </label>
                  <div class="card-item__content">
                    <label for="cardName" class="card-item__info">
                      <div class="card-item__holder">Card Holder</div>
                      <transition name="slide-fade-up">
                        <div class="card-item__name" v-if="cardName.length" key="1">
                          <transition-group name="slide-fade-right">
                            <span class="card-item__nameItem" v-for="(n, $index) in cardName.replace(/\s\s+/g, ' ')"
                              v-bind:key="$index + 1">{{ n }}</span>
                          </transition-group>
                        </div>
                        <div class="card-item__name" v-else key="2">Full Name</div>
                      </transition>
                    </label>
                    <div class="card-item__date" ref="cardDate">
                      <label for="cardMonth" class="card-item__dateTitle">Expires</label>
                      <label for="cardMonth" class="card-item__dateItem">
                        <transition name="slide-fade-up">
                          <span v-if="cardMonth" v-bind:key="cardMonth">{{ cardMonth }}</span>
                          <span v-else key="2">MM</span>
                        </transition>
                      </label>
                      /
                      <label for="cardYear" class="card-item__dateItem">
                        <transition name="slide-fade-up">
                          <span v-if="cardYear" v-bind:key="cardYear">{{ String(cardYear).slice(2, 4) }}</span>
                          <span v-else key="2">YY</span>
                        </transition>
                      </label>
                    </div>
                  </div>
                </div>
              </div>
              <div class="card-item__side -back">
                <div class="card-item__cover">
                  <img
                    v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + currentCardBackground + '.jpeg'"
                    class="card-item__bg">
                </div>
                <div class="card-item__band"></div>
                <div class="card-item__cvv">
                  <div class="card-item__cvvTitle">CVV</div>
                  <div class="card-item__cvvBand">
                    <span v-for="(n, $index) in cardCvv" :key="$index">
                      *
                    </span>
                  </div>
                  <div class="card-item__type">
                    <img
                      v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + getCardType + '.png'"
                      v-if="getCardType" class="card-item__typeImg">
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="card-form__inner">
            <div class="card-input">
              <label for="cardNumber" class="card-input__label">Número de tarjeta</label>
              <input type="text" class="card-input__input" id="cardNumber" v-mask="generateCardNumberMask"
                v-model="cardNumber" v-on:focus="focusInput" v-on:blur="blurInput" data-ref="cardNumber"
                autocomplete="off">
            </div>
            <div class="card-input">
              <label for="cardName" class="card-input__label">Titular de la tarjeta</label>
              <input type="text" id="cardName" class="card-input__input" v-model="cardName" v-on:focus="focusInput"
                v-on:blur="blurInput" data-ref="cardName" autocomplete="off">
            </div>
            <div class="card-form__row">
              <div class="card-form__col">
                <div class="card-form__group">
                  <label for="cardMonth" class="card-input__label">Fecha de expiración</label>
                  <select class="card-input__input -select" id="cardMonth" v-model="cardMonth" v-on:focus="focusInput"
                    v-on:blur="blurInput" data-ref="cardDate">
                    <option value="" disabled selected>Mes</option>
                    <option v-bind:value="n < 10 ? '0' + n : n" v-for="n in 12" v-bind:disabled="n < minCardMonth"
                      v-bind:key="n">
                      {{ n < 10 ? '0' + n : n }} </option>
                  </select>
                  <select class="card-input__input -select" id="cardYear" v-model="cardYear" v-on:focus="focusInput"
                    v-on:blur="blurInput" data-ref="cardDate">
                    <option value="" disabled selected>Año</option>
                    <option v-bind:value="$index + minCardYear" v-for="(n, $index) in 12" v-bind:key="n">
                      {{ $index + minCardYear }}
                    </option>
                  </select>
                </div>
              </div>
              <div class="card-form__col -cvv">
                <div class="card-input">
                  <label for="cardCvv" class="card-input__label">CVV</label>
                  <input type="text" class="card-input__input" id="cardCvv" v-mask="'####'" maxlength="4"
                    v-model="cardCvv" v-on:focus="flipCard(true)" v-on:blur="flipCard(false)" autocomplete="off">
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="checkOut">
        <div class="subtotal">
          <p>SUBTOTAL:</p>
          <p>$<span id="subtotal">{{ calculateSubtotal() }}</span></p>
        </div>
        <button v-if="calculateSubtotal() !== 0 && confirmateShooping === false"
          @click="confirmateShoppingCart()">Confirmar</button>
        <button v-if="confirmateShooping === true">Pagar</button>
      </div>
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
