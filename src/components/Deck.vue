<template>
  <!-------------------------------- DECK -------------------------------->
  <div class="deck col-12 col-md-4">
    <div class="classeCard">
      <p>DECK CHAMAN</p>
      <p>0/30</p>
    </div>
    <div class="deckListe" @drop="drop" @dragover="allowDrop">
      <div class="carteListe" v-for="el of deckList" :key="el.name">
        <p class="coutMana">{{ el.cost }}</p>
        <p class="nom">{{ el.name }}</p>
        <p class="nbrExemplaire">X{{el.copy}}</p>
      </div>
        <h1>coucou</h1>
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
      data.copy = 1;  //un exemplaire
      this.deckList.push(data);
      console.log(this.deckList);
    });
  },
  methods: {
    allowDrop(ev) {
      ev.preventDefault();
    },
    drop(ev) {
      ev.preventDefault();
      //Nom et cout viennent du dataset de CarteChoix
      let coutCarte = ev.dataTransfer.getData("cout");
      let nomCarte = ev.dataTransfer.getData("nom");

      //Si l'a carte n'est pas dans la liste du deck, on l'ajoute, sinon on met la copie à 1
      let indexTest = this.deckList.findIndex(obj => obj.name === nomCarte);
      if(indexTest === -1 ) {
        this.deckList.push({name:nomCarte,cost:coutCarte,copy:1});
      }else {
        this.deckList[indexTest].copy = 2;
      }
    },
  },
};
</script>

<style scoped></style>
