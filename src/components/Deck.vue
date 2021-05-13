<template>
  <!-------------------------------- DECK -------------------------------->
  <div class="deck col-12 col-md-4">
    <div class="classeCard">
      <p>DECK CHAMAN</p>
      <p>0/30</p>
    </div>
    <div class="deckListe">
      <div class="carteListe" v-for="el of deckList" :key="el.name">
        <p class="coutMana">{{el.cost}}</p>
        <p class="nom">{{el.name}}</p>
        <p class="nbrExemplaire">X1</p>
      </div>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      deckList: [],
    };
  },
  created() {
    bus.$on("choixCarte", (data) => {
      this.deckList.push(data);
    });
  },
};
</script>

<style scoped></style>
