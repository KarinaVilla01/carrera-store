<script>
import { ref } from "vue"
import Resume from "../src/components/Resume.vue"
import img1 from '../src/_assets/BackSupremo.png'
import img2 from '../src/_assets/BackExpress.png'
import img3 from '../src/_assets/BackBasico.png'
import img4 from '../src/_assets/BackUltra.png'
import img5 from '../src/_assets/BackSupremoSelect.png'
import img6 from '../src/_assets/BackUltraSelected.png'
import img7 from '../src/_assets/BackBasicoSelect.png'
import img8 from '../src/_assets/BackExpressSelected.png'

export default {
  setup() {
    const packages = ref([
      {
        _id: "612abcd1c4ce4c141237a356",
        image: img1,
        imageSelect: img5,
        name: "Supremo",
        pricePackage: 400,
        primeMembership: 1000,
        isSelected: false,
      },
      {
        _id: "612f1c4f30b90803837e7969",
        image: img4,
        imageSelect: img6,
        name: "Ultra",
        pricePackage: 300,
        primeMembership: 800,
        isSelected: false,
      },
      {
        _id: "612f067387e473107fda56b0",
        image: img3,
        imageSelect: img7,
        name: "Basico",
        pricePackage: 200,
        primeMembership: 650,
        isSelected: false,
      },
      {
        _id: "612f057787e473107fda56aa",
        image: img2,
        imageSelect: img8,
        name: "Express",
        pricePackage: 100,
        primeMembership: 500,
        isSelected: false,
      },
    ]);
    const showCart = ref(false);
    const options = ref([
      { id: 1, imgSrc: 'src/_assets/packageIco.png', value: 'Paquete', label: 'Paquete' },
      { id: 2, imgSrc: 'src/_assets/membresia.png', value: 'Membresía', label: 'Membresía' }
    ]);
    const messageTop = ref(false);
    const allwaysSelect = ref("option1");
    const allwaysSelect2 = ref("option2");
    const paymentCorrect = ref(false);
    const activatePopup = ref(false);
    const selectedOption = ref(null);
    const selectedPackageId = ref(null);
    const selectedPackage = ref(null)
    const confirmateShooping = ref(false);
    const confirmatePayment = ref(false);
    const confirmateCartPayment = ref(false);
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

    function selectOption(option) {
      selectedOption.value = option;
    };

    const optionImageSrc = (value) => {
      const option = options.value.find(o => o.value === value);
      return option ? option.imgSrc : '';
    };

    const selectPackage = (pack) => {
      selectedPackage.value = pack;
      selectedPackageId.value = pack._id;

      packages.value.forEach(p => {
        p.isSelected = p._id === selectedPackageId.value;
      });

    };

    const activatedPopup = () => {
      activatePopup.value = !activatePopup.value;
      confirmatePayment.value = !confirmatePayment.value;
    }

    const confirmateShoppingCart = () => {
      confirmateShooping.value = !confirmateShooping.value;
    }

    const confirmatePaymentMethod = () => {
      confirmateCartPayment.value = !confirmateCartPayment.value;
      messageTop.value = !messageTop.value;
      confirmatePayment.value = !confirmatePayment.value;
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
      confirmatePayment,
      confirmateCartPayment,
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
      optionImageSrc,
      changeMesage,
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
      confirmatePaymentMethod
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
      <div class="listCart" v-if="confirmateShooping === false && confirmateCartPayment === false">
        <h5>Elije entre <span>"Paquete"</span>
          para servicio único <br> o <span>"Membresía"</span>
          para beneficios continuos. </h5>
        <div class="paqMem">
          <h4>PAQUETE</h4>
          <h4>MEMBRESÍA</h4>
        </div>
        <div class="selection">
          <div v-for="option in options" :key="option"
            :class="{ 'selection-btn': true, 'selected': selectedOption === option.value }"
            @click="selectOption(option.value)">
            <img class="img-pagMem" :src="option.imgSrc" alt="imagen">
            <input type="radio" name="radio-card" v-model="selectedOption" :value="option.value" />
          </div>
        </div>
        <p v-if="selectedOption === 'Membresía'" class="parra">Tu <span>membresía</span> se renovará mensualmente de forma
          atomática.</p>
        <div v-if="selectedOption">
          <h4>SELECCIONA TU SERVICIO PREFERIDO</h4>
          <ul class="listCartInto">
            <li v-for="pack in packages" :key="pack._id" :style="{ backgroundImage: 'url(' + pack.image + ')' }"
              @click="selectPackage(pack)" :class="{ 'selectedOption': pack.isSelected }">
              <a :href="'#/package/' + pack._id">
                <div class="packLetters">{{ pack.name }}</div>
                <div v-if="selectedOption === 'Paquete'" class="packLetters">${{ pack.pricePackage }} <span>.00</span>
                </div>
                <div v-if="selectedOption === 'Membresía'" class="packLetters">${{ pack.primeMembership }}<span>.00</span>
                </div>
              </a>
            </li>
          </ul>
        </div>
      </div>
      <div v-if="confirmateShooping === true && confirmateCartPayment === false && activatePopup === false">
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
                    <img src="../src/_assets/chip.png" class="card-item__chip">
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
                      <div class="card-item__holder">Titular</div>
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
                      <label for="cardMonth" class="card-item__dateTitle">Expira</label>
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
              <label for="cardName" class="card-input__label">Titular de la tarjeta</label>
              <div class="input-container">
                <i class="fa fa-user"></i>
                <input type="text" id="cardName" class="card-input__input" v-model="cardName" v-on:focus="focusInput"
                  v-on:blur="blurInput" data-ref="cardName" autocomplete="off" placeholder="Nombre completo del titular">
              </div>
            </div>
            <div class="card-input">
              <label for="cardNumber" class="card-input__label">Número de tarjeta</label>
              <div class="input-container">
                <i class="fa-regular fa-credit-card"></i>
                <input type="text" class="card-input__input" id="cardNumber" v-mask="generateCardNumberMask"
                  v-model="cardNumber" v-on:focus="focusInput" v-on:blur="blurInput" data-ref="cardNumber"
                  placeholder="Número de tarjeta" autocomplete="off">
              </div>
            </div>
            <div class="card-form__row">
              <div class="card-form__col">
                <div class="card-form__group">
                  <label for="cardMonth" class="card-input__label">Fecha de expiración</label>
                  <div class="input-container">
                    <i class="fa-solid fa-calendar-days"></i>
                    <select class="card-input__input -select" id="cardMonth" v-model="cardMonth" v-on:focus="focusInput"
                      v-on:blur="blurInput" data-ref="cardDate">
                      <option value="" disabled selected>Mes</option>
                      <option v-bind:value="n < 10 ? '0' + n : n" v-for="n in 12" v-bind:disabled="n < minCardMonth"
                        v-bind:key="n">
                        {{ n < 10 ? '0' + n : n }} </option>
                    </select>

                  </div>
                  <div class="input-container">
                    <i class="fa-regular fa-calendar"></i>
                    <select class="card-input__input -select" id="cardYear" v-model="cardYear" v-on:focus="focusInput"
                      v-on:blur="blurInput" data-ref="cardDate">
                      <option value="" disabled selected>Año</option>
                      <option v-bind:value="$index + minCardYear" v-for="(n, $index) in 12" v-bind:key="n">
                        {{ $index + minCardYear }}
                      </option>
                    </select>
                  </div>
                </div>
              </div>
              <div class="card-form__col -cvv">
                <div class="card-input">
                  <label for="cardCvv" class="card-input__label">CVV</label>
                  <div class="input-container">
                    <i class="fa-solid fa-lock"></i>
                    <input type="text" class="card-input__input" id="cardCvv" v-mask="'####'" maxlength="4"
                      v-model="cardCvv" v-on:focus="flipCard(true)" v-on:blur="flipCard(false)" autocomplete="off"
                      placeholder="***">
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="textConfirmate"
        v-if="confirmateCartPayment === true && activatePopup === false && paymentCorrect === false">
        <Resume />
        <h4>TIPO: {{ selectedOption }}</h4>
        <div class="type">
          <img v-if="selectedOption === 'Paquete'" class="imgPagOMem" :src="optionImageSrc('Paquete')"
            alt="Imagen de Paquete">
          <img v-if="selectedOption === 'Membresía'" class="imgPagOMem" :src="optionImageSrc('Membresía')"
            alt="Imagen de Membresía">
          <input type="radio" name="radio-card" value="option1" v-model="allwaysSelect" />
        </div>
        <h4 v-if="selectedOption === 'Paquete'">PAQUETE ELEGIDO:</h4>
        <h4 v-if="selectedOption === 'Membresía'">MEMBRESÍA ELEGIDA:</h4>
        <div class="type w-2">
          <div v-if="selectedPackage" class="type2"
            :style="{ backgroundImage: 'url(' + selectedPackage.imageSelect + ')' }">
            <p class="packLetters">{{ selectedPackage.name }}</p>
            <p v-if="selectedOption === 'Paquete'" class="packLetters">${{ selectedPackage.pricePackage }}
              <span>.00</span>
            </p>
            <p v-if="selectedOption === 'Membresía'" class="packLetters">${{ selectedPackage.primeMembership
            }}<span>.00</span></p>
          </div>
          <input type="radio" name="radio-card2" value="option2" style="left: -3%;" v-model="allwaysSelect2" />
        </div>
        <h4>CON CARGO A:</h4>
        <div class="card-item">
          <div class="card-item__side -front">
            <div class="card-item__focus"></div>
            <div class="card-item__cover">
              <img
                v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + currentCardBackground + '.jpeg'"
                class="card-item__bg">
            </div>

            <div class="card-item__wrapper">
              <div class="card-item__top">
                <img src="../src/_assets/chip.png" class="card-item__chip">
                <div class="card-item__type">
                  <transition name="slide-fade-up">
                    <img
                      v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + getCardType + '.png'"
                      v-if="getCardType" v-bind:key="getCardType" alt="" class="card-item__typeImg">
                  </transition>
                </div>
              </div>
              <label class="card-item__number s-cursor">
                <span>
                  {{ cardNumber }}
                </span>
              </label>
              <div class="card-item__content">
                <label for="cardName" class="card-item__info s-cursor">
                  <div class="card-item__holder">Titular</div>
                  <div class="card-item__name">
                    {{ cardName }}
                  </div>
                </label>
                <div class="card-item__date" ref="cardDate">
                  <label for="cardMonth" class="card-item__dateTitle s-cursor">Expira</label>
                  <label for="cardMonth" class="card-item__dateItem s-cursor">
                    <span>{{ cardMonth }}</span>
                    /
                  </label>
                  <label for="cardYear" class="card-item__dateItem">
                    <span>{{ String(cardYear).slice(2, 4) }}</span>
                  </label>
                </div>
              </div>
            </div>
          </div>
        </div>
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
        <div>
          <img src="./_assets/logoCarrera.png" alt="">
        </div>
        <div>
          <img src="./_assets/Confirmation Ico.png" alt="">
        </div>
        <h1>${{ calculateSubtotal() }}.00</h1>
        <h4> ¡Pago Exitoso!</h4>
        <div class="card-payment">
          <div>
            <div class="intoCardPayment">
              <h4>CONCEPTO: </h4>
              <div>
                <p v-if="selectedOption === 'Membresía'"> Membresía Mensual.</p>
                <p v-if="selectedOption === 'Paquete'"> Paquete.</p>
              </div>
            </div>
          </div>
          <div class="seccion1">
            <div>
              <p>Pagaste a:</p>
              <h4>CARRERA CARWASH</h4>
            </div>
            <img class="flagsImg" src="./_assets/flags.png" alt="">
          </div>
          <div class="seccion1">
            <div>
              <p>Servicio:</p>
              <h4>{{ selectedPackage.name }}</h4>
            </div>
            <img class="flagsImg" :src="optionImageSrc('Paquete')" alt="">
          </div>
          <div class="seccion1">
            <div>
              <p>Método de pago:</p>
              <h4>{{ cardNumber }}</h4>
            </div>
            <img class="flagsImg" src="./_assets/visa-logo.png" alt="">
          </div>
          <div class="total">
            <h4>TOTAL: <span>${{ calculateSubtotal() }}.00</span></h4>
          </div>
        </div>
        <button @click="toggleCart">ACEPTAR</button>
      </div>
      <div v-if="paymentCorrect === false" class="checkOut">
        <div class="subtotal">
          <p>SUBTOTAL:</p>
          <p>$<span id="subtotal">{{ calculateSubtotal() }}.00</span></p>
        </div>
        <button v-if="calculateSubtotal() !== 0 && confirmateShooping === false"
          @click="confirmateShoppingCart()">Confirmar pedido</button>
        <button v-if="confirmateShooping === true && confirmateCartPayment === false && activatePopup === false"
          @click="confirmatePaymentMethod()">REALIZAR EL PAGO</button>
        <button v-if="confirmatePayment === true && activatePopup === false && selectedOption === 'Membresía'"
          @click="activatedPopup">CONFIRMAR PAGO</button>
        <button v-if="confirmatePayment === true && selectedOption === 'Paquete'" @click="autorizePayment">CONFIRMAR
          PAGO</button>
        <div v-if="confirmatePayment === true && selectedOption === 'Membresía'" class="terminos">
          <p>Al proceder, aceptas los Términos y Condiciones y <br> reconoces que tu membresía
            <span>se renovará <br>automáticamente con cargo mensual.</span>
          </p>
        </div>
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
