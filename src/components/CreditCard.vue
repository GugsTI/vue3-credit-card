<template>
  <div class="card" :class="{ flip: backface }">
    <div class="front">
      <div class="card-top">
        <img src="../assets/image/chip.svg" alt="chip logo" />
        <img v-if="type" :src="imgUrl" alt="" />
      </div>
      <div class="card-number">{{ card.number || "**** **** **** ****" }}</div>
      <div class="card-bottom">
        <div>
          <div class="key">Nome no cartão</div>
          <div class="value">{{ card.name || "***" }}</div>
        </div>
        <div>
          <div class="key">Data de Expiração</div>
          <div class="value">{{ card.expire || "**/**" }}</div>
        </div>
      </div>
    </div>
    <div class="back">
      <div class="card-back">
        CVV <em>{{ card.cvv || "***" }}</em>
      </div>
    </div>
  </div>
  <br />
  <input
    v-maska="'#### #### #### ####'"
    type="text"
    v-model="card.number"
    placeholder="Número do cartão"
  /><br />
  <br />
  <input type="text" v-model="card.name" placeholder="Nome no cartão" /><br />
  <br />
  <input
    type="text"
    v-maska="'##/##'"
    v-model="card.expire"
    placeholder="Data de expirar"
  /><br />
  <br />
  <input
    @focus="backface = true"
    @blur="backface = false"
    type="text"
    v-maska="'###'"
    v-model="card.cvv"
    placeholder="CVV"
  /><br />
</template>

<script setup>
import { ref, reactive, computed, watchEffect } from "vue";
import creditCardType from "credit-card-type";

let backface = ref(false);

let card = reactive({
  number: "",
  name: "",
  expire: "",
  cvv: "",
});

const type = computed(() => {
  return card.number ? creditCardType(card.number)?.[0]?.type : false;
});
const imgUrl = computed(() => {
  return new URL(`../assets/image/${type.value}.svg`, import.meta.url);
});
</script>

<style>
.card {
  width: 352px;
  height: 223px;
  position: relative;
  perspective: 800px;
}
.card .front,
.card .back {
  box-sizing: border-box;
  width: inherit;
  height: inherit;
  background: linear-gradient(31.58deg, #93278f -2.49%, #29abe2 67.92%);
  border-radius: 15px;
  position: absolute;
  top: 0;
  left: 0;
  backface-visibility: hidden;
  transition: 1s all;
  padding: 31px 27px;
  display: flex;
  flex-direction: column;
}
.card .back {
  transform: rotateY(180deg);
}

.card .front .card-top {
  display: flex;
  justify-content: space-between;
}
.card .front .card-number {
  font-size: 24px;
  font-family: monospace;
  letter-spacing: -3px;
  color: #fff;
  margin-top: 25px;
}
.card .front .card-bottom {
  margin-top: auto;
  display: flex;
  justify-content: space-between;
  color: #fff;
  font-weight: 500;
}

.card .front .card-bottom .key {
  font-size: 12px;
  letter-spacing: -0.3px;
  opacity: 0.7;
  margin-bottom: 5px;
}
.card .front .card-bottom .value {
  font-size: 18px;
}

.card .back .card-back {
  background: #fff;
  padding: 20px;
  margin-top: auto;
  display: flex;
  justify-content: end;
}
.card .back .card-back em {
  font-weight: bold;
  margin-left: 15px;
}

.card:hover .back,
.card.flip .back {
  transform: rotateY(0);
}
.card:hover .front,
.card.flip .front {
  transform: rotateY(-180deg);
}
</style>
￼