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

    <!-- MODALE -->
    <div class="popUp" :class="{ invisible: popUpInvisible }" @click="removePopUp">
      <div class="textBox">
        <span class="croix">❌</span>
        <div class="contenu">
          <h3>Votre deck est plein !</h3>
          <p>Vous devez d'abord retirer une carte avant d'en ajouter une.</p>
        </div>
      </div>
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
      popUpInvisible: true,
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
      } else {
        this.popUpInvisible = false;
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
    },
    /**
     * Fonction d'envoi de l'information qu'une carte n'est plus disponible
     * (nombre maximum dans un deck est atteint)
     */
    envoiCartePlusDispo(nomCarte) {
      bus.$emit("cartePlusDispo", nomCarte);
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
    removePopUp(e) {
      if (e.target.classList.contains("popUp") || e.target.classList.contains("croix")) {
        this.popUpInvisible = true;
      }
    },
    triDeckListe() {
      this.deckList.sort((a, b)=>  a.cost- b.cost || a.name.localeCompare(b.name));
    }
  },
};
</script>

<style scoped>
.popUp {
  position: fixed;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.8);
  height: 100vh;
  width: 100%;
  transition: all 800ms;
}
.textBox {
  border: 2px solid black;
  background-color: #eee;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  padding: 5%;
  min-width: 300px;
  color: black;
  font-size:1.2em;
}
.contenu {
  width: 90%;
  margin: auto;
}

.croix {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 1.2rem;
  cursor: pointer;
}

.invisible {
  display: none;
}
</style>
