<template>
  <!-------------------------------- DECK -------------------------------->
  <div class="deck col-12 col-md-4">
    <div class="classeCard">
      <p>DECK CHAMAN</p>
      <p>{{ nbrCarte }}/30</p>
    </div>
    <div class="deckListe" @drop="drop" @dragover="allowDrop">
      <div class="carteListe" v-for="el of deckList" :key="el.name" @click="removeCard">
        <p class="coutMana">{{ el.cost }}</p>
        <p class="nom">{{ el.name }}</p>
        <p class="nbrExemplaire">X{{ el.copy }}</p>
      </div>
      <h1 v-if="this.deckList.length === 0" class="info-deck">Cliquez sur les cartes ou faites les glisser pour les ajouter</h1>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      deckList: [],
      nbrCarte: 0,
    };
  },
  created() {
    bus.$on("choixCarte", (data) => {
      data.copy = 1; //un exemplaire
      this.alreadyInDeck(data.name, [data]);
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
      let rareteCarte = ev.dataTransfer.getData("rarete");
      this.alreadyInDeck(nomCarte, [{ name: nomCarte, cost: coutCarte, rarity: rareteCarte, copy: 1 }]);
    },
    alreadyInDeck(nomdeCarte, [carteApusher]) {
      this.nbrCarte++;
      let indexTest = this.deckList.findIndex((obj) => obj.name === nomdeCarte);
      //Si l'a carte n'est pas dans la liste du deck, on l'ajoute, sinon on met la copie à 1 (sauf légendaire)
      if (indexTest === -1) {
        this.deckList.push(carteApusher);
        if (carteApusher.rarity === "Legendary") this.envoiCartePlusDispo(carteApusher.name); //la carte n'est plus dispo après 1exemplaire si légendaire
      } else {
        this.deckList[indexTest].copy = 2;
        this.envoiCartePlusDispo(this.deckList[indexTest].name);   //on envoi le nom de la carte qui n'est plus dispo
      }
    },
    removeCard(e) {
      let nomCarte;
      if (e.target.parentNode.classList.contains("carteListe")) {
        nomCarte = e.target.parentNode.childNodes[1].textContent;  // le 2ème enfant du parent de l'élément sélectionné (évite problème si click sur cout ou nbr Exemplaire)
      } else {
        nomCarte = e.target.childNodes[1].textContent; //si on clique sur les carteListe sans cliquer sur élément qui la compose
      }
      let Carte = this.deckList.find((el) => el.name === nomCarte); //la carte dans la deckList
      let indexTest = this.deckList.findIndex((obj) => obj.name === nomCarte);
      if (Carte.copy === 2) {
        this.deckList[indexTest].copy = 1;
        this.envoiCarteReDispo(this.deckList[indexTest].name); // envoi info que carte de nouveau disponible
        this.$forceUpdate();
      } else {
        if(this.deckList[indexTest].rarity === "Legendary") this.envoiCarteReDispo(this.deckList[indexTest].name); //si légendaire, dispo seulement quand plus d'exemplaire
        this.deckList.splice(indexTest, 1);
      }
      this.nbrCarte--;
    },
    envoiCartePlusDispo(nomCarte) {
      bus.$emit("cartePlusDispo", nomCarte);
    },
    envoiCarteReDispo(nomCarte) {
      bus.$emit("carteReDispo", nomCarte);
    },
  },
};
</script>

<style scoped></style>
