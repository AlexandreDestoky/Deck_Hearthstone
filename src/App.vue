<template>
  <div id="app">
    <h1>Constructeur de Deck Hearthstone</h1>
    <div class="container">
      <ChoixPerso />
      <BarreTri />
      <div class="creation">
        <CarteChoix />
        <Deck />
      </div>
    </div>
    <PopUpInfo />
    <!-------------------------------- FOOTER -------------------------------->
    <footer>
      <p>Cette Application est réalisé par :</p>
      <p>Alexandre Destoky</p>
    </footer>
  </div>
</template>

<script>
import ChoixPerso from "./components/ChoixPerso.vue";
import BarreTri from "./components/BarreTri.vue";
import CarteChoix from "./components/CarteChoix.vue";
import Deck from "./components/Deck.vue";
import PopUpInfo from "./components/PopUpInfo.vue";

export default {
  name: "App",
  components: {
    ChoixPerso,
    BarreTri,
    CarteChoix,
    Deck,
    PopUpInfo
  },
};
</script>

<style>
@import "./assets/css/style.css";
</style>
