<template>
  <!-------------------------------- DECK -------------------------------->
  <div class="deck col-12 col-md-4">
    <div class="classeCard">
      <p>DECK CHAMAN</p>
      <p>0/30</p>
    </div>
    <div class="deckListe">
      <p>Salut</p>
      <p>{{test.name + test.id}} </p>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      test : undefined
    }
  },
  created() {
    bus.$on("choixCarte",(data)=> {
      this.test = data;
    })
  }
};
</script>

<style scoped></style>
