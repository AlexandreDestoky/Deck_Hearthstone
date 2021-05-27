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
        <p class="nom" :style="{ color: changementCouleur(el.rarity) }">{{ el.name }}</p>
        <p class="nbrExemplaire">X{{ el.copy }}</p>
      </div>
      <!-- Si il n'y a pas de carte dans le deck on donne les instructions -->
      <h1 v-if="this.deckList.length === 0" class="info-deck">Cliquez sur les cartes ou faites les glisser pour les ajouter</h1>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      deckList: [], //Liste des cartes dans deck
      nbrCarte: 0, //Compteur
      popUpTitre: "Votre deck est plein !",
      popUpTexte: "Vous devez d'abord retirer une carte avant d'en ajouter une.",
    };
  },
  created() {
    //Quand on click sur une carte (dans la liste de carte)
    bus.$on("choixCarte", (data) => {
      data.copy = 1; //un exemplaire
      this.alreadyInDeck(data.name, [data]);
    });
  },
  methods: {
    /**
     * Empeche comportement par défaut d'empecher le drop
     */
    allowDrop(ev) {
      ev.preventDefault();
    },
    /**
     * Lorsque l'on drop une carte dans la partie deck
     */
    drop(ev) {
      ev.preventDefault();
      //Nom,cout et rareté viennent du dataset de CarteChoix
      let coutCarte = ev.dataTransfer.getData("cout");
      let nomCarte = ev.dataTransfer.getData("nom");
      let rareteCarte = ev.dataTransfer.getData("rarete");
      this.alreadyInDeck(nomCarte, [{ name: nomCarte, cost: coutCarte, rarity: rareteCarte, copy: 1 }]);
    },
    /**
     * Fonction de test si une carte est déja dans le deck
     */
    alreadyInDeck(nomdeCarte, [carteApusher]) {
      if (this.nbrCarte < 30) {
        this.nbrCarte++; //compteur de carte augmente
        let indexTest = this.deckList.findIndex((obj) => obj.name === nomdeCarte);
        //Si l'a carte n'est pas dans la liste du deck, on l'ajoute, sinon on met la copie à 1 (sauf légendaire)
        if (indexTest === -1) {
          this.deckList.push(carteApusher);
          this.triDeckListe();
          if (carteApusher.rarity === "Legendary") this.envoiCartePlusDispo(carteApusher.name); //la carte n'est plus dispo après 1exemplaire si légendaire
        } else {
          this.deckList[indexTest].copy = 2;
          this.envoiCartePlusDispo(this.deckList[indexTest].name); //on envoi le nom de la carte qui n'est plus dispo
        }
        this.envoiNbrCartes(); // on informe qu'il y a des cartes
      } else {
        this.openPopUp();
      }
    },
    /**
     * Fonction Pour enlever une carte du deck
     */
    removeCard(e) {
      this.nbrCarte--; //compteur de carte diminue
      let nomCarte;
      if (e.target.parentNode.classList.contains("carteListe")) {
        nomCarte = e.target.parentNode.childNodes[1].textContent; // le 2ème enfant du parent de l'élément sélectionné (évite problème si click sur cout ou nbr Exemplaire)
      } else {
        nomCarte = e.target.childNodes[1].textContent; //si on clique sur les .carteListe sans cliquer sur élément qui la compose
      }
      let Carte = this.deckList.find((el) => el.name === nomCarte); //la carte dans la deckList
      let indexTest = this.deckList.findIndex((obj) => obj.name === nomCarte);
      if (Carte.copy === 2) {
        this.deckList[indexTest].copy = 1;
        this.envoiCarteReDispo(this.deckList[indexTest].name); // envoi info que carte de nouveau disponible
        this.$forceUpdate(); //force la mise a jour de l'affichage pour voir suppression
      } else {
        if (this.deckList[indexTest].rarity === "Legendary") this.envoiCarteReDispo(this.deckList[indexTest].name); //si légendaire, dispo seulement quand plus d'exemplaire
        this.deckList.splice(indexTest, 1); // on enleve la carte de la decklist
      }
      this.envoiNbrCartes();
    },
    /**
     * Fonction d'envoi de l'information qu'une carte n'est plus disponible
     * (nombre maximum dans un deck est atteint)
     */
    envoiCartePlusDispo(nomCarte) {
      bus.$emit("cartePlusDispo", nomCarte);
    },
    envoiNbrCartes() {
      bus.$emit("nbrCartes", this.deckList.length > 0);
    },
    /**
     * Fonction d'envoi de l'information qu'une carte est de nouveau disponible
     * (nombre maximum dans un deck n'est plus atteint)
     */
    envoiCarteReDispo(nomCarte) {
      bus.$emit("carteReDispo", nomCarte);
    },
    changementCouleur(rarete) {
      let couleur;
      switch (rarete) {
        case "Legendary":
          couleur = "#fdc500";
          break;
        case "Epic":
          couleur = "#ce11ce";
          break;
        case "Rare":
          couleur = "#4899f4";
          break;
        default:
          couleur = "#eee";
          break;
      }
      return couleur;
    },
    // removePopUp(e) {
    //   if (e.target.classList.contains("popUp") || e.target.classList.contains("croix")) {
    //     this.popUpInvisible = true;
    //   }
    // },
    triDeckListe() {
      this.deckList.sort((a, b) => a.cost - b.cost || a.name.localeCompare(b.name));
    },
    openPopUp() {
      bus.$emit("popUpInvisible", [this.popUpTitre, this.popUpTexte]);
    },
  },
};
</script>

<style scoped></style>
