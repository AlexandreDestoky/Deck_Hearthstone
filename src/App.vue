<template>
  <div id="app">
    <h1>Hearthstone Deck Builder</h1>
    <div class="container">
      <ChoixPerso />
      <BarreTri />
      <div class="creation">
        <CarteChoix />
        <Deck />
      </div>
    </div>
    <PopUpInfo />
    <PopUpChoice />
    <!-------------------------------- FOOTER -------------------------------->
    <footer>
      <p>This Application is produced by :</p>
      <p>Alexandre Destoky</p>
    </footer>
  </div>
</template>

<script>
import ChoixPerso from "./components/ChoixPerso.vue";
import BarreTri from "./components/BarreTri.vue";
import CarteChoix from "./components/CarteChoix.vue";
import Deck from "./components/Deck.vue";
import PopUpInfo from "./components/PopUp/PopUpInfo.vue";
import PopUpChoice from "./components/PopUp/PopUpChoice.vue";

export default {
  name: "App",
  components: {
    ChoixPerso,
    BarreTri,
    CarteChoix,
    Deck,
    PopUpInfo,
    PopUpChoice,
  },
};
</script>

<style>
@import "./assets/css/style.css";
</style>
