<template>
  <!-------------------------------- DECK -------------------------------->
  <div class="deck col-12 col-md-4">
    <div class="classeCard">
      <p>{{ classeChoisie === undefined ? "CLASS" : formatageClasse(classeChoisie) }} DECK</p>
      <p>{{ nbrCarte }}/30</p>
    </div>
    <div class="deckListe" @drop="drop" @dragover="allowDrop">
      <div class="carteListe" v-for="carte of deckListe" :key="carte.name" @click="supprimeCarte">
        <p class="coutMana">{{ carte.cost }}</p>
        <p class="nom" :style="{ color: changementCouleur(carte.rarity) }">{{ carte.name }}</p>
        <p class="nbrExemplaire">X{{ carte.copy }}</p>
      </div>
      <!-- Si il n'y a pas de carte dans le deck on donne les instructions -->
      <h1 v-if="this.deckListe.length === 0" class="info-deck">Click or drag cards to add them</h1>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      deckListe: [], //Liste des cartes dans deck
      nbrCarte: 0, //Compteur
      popUpInfoTitre: "Votre deck est plein !",
      popUpInfoTexte: "Vous devez d'abord retirer une carte avant d'en ajouter une.",
      classeChoisie: undefined,
    };
  },
  created() {
    /**
     * Réception de l'évenement "choixCarte"
     * Quand on clique sur une carte (dans la liste de carte)
     */
    bus.$on("choixCarte", (data) => {
      data.copy = 1; // on met un exemplaire par défaut
      this.ajouteCarte(data.name, [data]);
    });
    /**
     * Réception de l'évenement "vidageDeck"
     * Si true on vide la deckListe et remet le compteur à 0
     */
    bus.$on("vidageDeck", (data) => {
      if (data) {
        this.deckListe = [];
        this.nbrCarte = 0;
      }
    });
    //On donne à classeChoisie la classe choisie dans le composant choixPerso
    /**
     * Réception de l'évenement "choixClasse"
     * Utiliser pour actualiser le nom de la classe affiché dans le deck
     */
    bus.$on("choixClasse", (data) => {
      this.classeChoisie = data;
    });
  },
  mounted() {
    /**
     * Si il y a un localStorage pour la deckListe
     * On effectue les changement (remplissage deck + compteur)
     */
    if (localStorage.getItem("deckListe")) {
      try {
        this.deckListe = JSON.parse(localStorage.getItem("deckListe"));
        this.envoiNbrCartes();
        // Compteur devient le nombre de copie dans le deck
        this.nbrCarte = this.deckListe.reduce(function(a, b) {
          return a + b.copy;
        }, 0);
      } catch (e) {
        localStorage.removeItem("deckListe");
      }
    }
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
      this.ajouteCarte(nomCarte, [{ name: nomCarte, cost: coutCarte, rarity: rareteCarte, copy: 1 }]);
    },
    /**
     * Fonction de test si une carte est déja dans le deck
     */
    ajouteCarte(nomdeCarte, [carteApusher]) {
      if (this.nbrCarte < 30) {
        this.nbrCarte++; //compteur de carte augmente
        let indexTest = this.deckListe.findIndex((obj) => obj.name === nomdeCarte);
        //Si l'a carte n'est pas dans la liste du deck, on l'ajoute, sinon on met la copie à 1 (sauf légendaire)
        if (indexTest === -1) {
          this.deckListe.push(carteApusher);
          this.trideckListe();
          localStorage.setItem("deckListe", JSON.stringify(this.deckListe)); //LocalStorage => on actualise la deckListe
          if (carteApusher.rarity === "Legendary") this.envoiCartePlusDispo(carteApusher.name); //la carte n'est plus dispo après 1 exemplaire si légendaire
        } else {
          this.deckListe[indexTest].copy = 2;
          localStorage.setItem("deckListe", JSON.stringify(this.deckListe)); //LocalStorage => on actualise la deckListe
          this.envoiCartePlusDispo(this.deckListe[indexTest].name); //la carte n'est plus dispo après 2 exemplaire
        }
        this.envoiNbrCartes(); // on informe qu'il y a des cartes
      } else {
        this.openPopUpInfo(); //Lance ouverture du popUp si on veut ajouter une carte avec déjà 30 cartes dans deck
      }
    },
    /**
     * Fonction Pour enlever une carte du deck
     */
    supprimeCarte(e) {
      this.nbrCarte--; //compteur de carte diminue
      let nomCarte;
      if (e.target.parentNode.classList.contains("carteListe")) {
        nomCarte = e.target.parentNode.childNodes[1].textContent; // le 2ème enfant du parent de l'élément sélectionné (évite problème si click sur cout ou nbr Exemplaire)
      } else {
        nomCarte = e.target.childNodes[1].textContent; //si on clique sur les .carteListe sans cliquer sur élément qui la compose
      }

      let Carte = this.deckListe.find((el) => el.name === nomCarte); //la carte dans la deckListe
      let indexTest = this.deckListe.findIndex((obj) => obj.name === nomCarte);

      if (Carte.copy === 2) {
        this.deckListe[indexTest].copy = 1;
        localStorage.setItem("deckListe", JSON.stringify(this.deckListe)); //LocalStorage => on actualise la deckListe
        this.envoiCarteReDispo(this.deckListe[indexTest].name); // envoi info que carte de nouveau disponible
        this.$forceUpdate(); //force la mise a jour de l'affichage pour voir suppression
      } else {
        if (this.deckListe[indexTest].rarity === "Legendary") this.envoiCarteReDispo(this.deckListe[indexTest].name); //si légendaire, dispo seulement quand plus d'exemplaire
        this.deckListe.splice(indexTest, 1); // on enleve la carte de la deckListe
        localStorage.setItem("deckListe", JSON.stringify(this.deckListe)); //LocalStorage => on actualise la deckListe
      }
      this.envoiNbrCartes(); //Envoi info de si le deck est vide ou non
    },
    /**
     * Envoi de l'évenement "cartePlusDispo"
     * Fonction d'envoi de l'information qu'une carte n'est plus disponible
     * (nombre maximum dans un deck est atteint)
     */
    envoiCartePlusDispo(nomCarte) {
      bus.$emit("cartePlusDispo", nomCarte);
    },
    /**
     * Envoi de l'évenement "carteReDispo"
     * Fonction d'envoi de l'information qu'une carte est de nouveau disponible
     * (nombre maximum dans un deck n'est plus atteint)
     */
    envoiCarteReDispo(nomCarte) {
      bus.$emit("carteReDispo", nomCarte);
    },
    /**
     * Envoi de l'évenement "nbrCartes"
     * On envoi true si le deck est vide, false si il ne l'est pas
     */
    envoiNbrCartes() {
      bus.$emit("nbrCartes", this.deckListe.length === 0);
    },
    /**
     * Envoi de l'évenement "popUpInfoVisible"
     * On envoi le titre et texte que l'on veut dans le popUp
     */
    openPopUpInfo() {
      bus.$emit("popUpInfoVisible", [this.popUpInfoTitre, this.popUpInfoTexte]);
    },
    /**
     * Fonction de changement de la couleur du nom de la carte dans le deck
     * @return Renvoi la couleur selon la rareté
     */
    changementCouleur(rarete) {
      switch (rarete) {
        case "Legendary":
          return "#fdc500";
        case "Epic":
          return "#ce11ce";
        case "Rare":
          return "#4899f4";
        default:
          return "#eee";
      }
    },
    /**
     * Fonction de tri de la deck liste par cout en mana puis par ordre alphabétique de non
     */
    trideckListe() {
      this.deckListe.sort((a, b) => a.cost - b.cost || a.name.localeCompare(b.name));
    },
    /**
     * Fonction de formatage de la classe reçue
     * @return la classe à laquelle on a enlevé les underscore et mise en majuscule
     */
    formatageClasse(classe) {
      return classe.replace("_", " ").toUpperCase();
    },
  },
};
</script>

<style scoped></style>
