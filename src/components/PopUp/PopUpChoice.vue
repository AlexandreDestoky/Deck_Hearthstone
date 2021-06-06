<template>
  <div class="popUp" :class="{ invisible: popUpInvisible }" @click="dissimulePopUp">
    <div class="textBox">
      <div class="contenu">
        <h3>{{titre}}</h3>
        <p>{{texte}}</p>
      </div>
      <div class="boutons">
        <button class="valider" @click="viderDeck">OK</button>
        <button class="annuler">ANNULER</button>
      </div>
    </div>
  </div>
</template>

<script>
import { bus } from "../../main";
export default {
  data() {
    return {
      popUpInvisible: true,
      titre: "",
      texte: "",
      nomNouvelleClasse : undefined
    };
  },
  created() {
    /**
     * Réception de l'évenement "popUpChoiceVisible"
     * Affiche le popUp en lui donnant les infos dont il a besoin
     */
    bus.$on("popUpChoiceVisible", (data) => {
      //On attribue les données reçue
      this.titre = data[0];
      this.texte = data[1];
      this.nomNouvelleClasse = data[2];
      // On rend le popUp visible
      this.popUpInvisible = false;
    });
  },
  methods: {
    /**
     * Fonction de dissimulation du popUp
     */
    dissimulePopUp(e) {
      // Le popUp disparait si on clique sur le bouton annuler ou en dehors du popUp
      if (e.target.classList.contains("popUp") || e.target.classList.contains("annuler")) {
        this.popUpInvisible = true;
      }
    },
    /**
     * Fonction de vidage du deck lors de changement de classe (avec carte dans deck)
     */
    viderDeck() {
      bus.$emit("vidageDeck",true); // on informe le deck qu'il peut vider le deck
      bus.$emit("changementClasse",this.nomNouvelleClasse); // on informe ChoixPerso que l'on veut changer de classe
      bus.$emit("vidagePlusDispo", true); // on informe carteChoix que toutes les cartes sont redispo
      this.popUpInvisible = true; // dissimulation du popUp
      localStorage.removeItem("deckListe"); //LocalStorage => ont supprime la deckListe
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
  border: 4px solid grey;
  background-color: #eee;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  padding: 1.3em 1em;
  min-width: 300px;
  color: black;
  font-size: 1.2em;
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

.boutons {
  display: flex;
  justify-content: center;
  margin-top: 3em;
}

.boutons button {
  outline: none;
  padding: 0.2em 1em;
  border: 2px solid black;
  border-radius: 20px;
  margin: 0 0.5em;
  width: 40%;
  text-align: center;
  text-decoration: none;
  color: #eee;
}

.valider {
  background-color: green;
}

.valider:hover {
  background-color: #0b380b;
  box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.336);
}

.boutons button:hover {
  box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.336);
}
.annuler {
  background-color: firebrick;
  display: table;
}
.annuler:hover {
  background-color: #6f0a0a;
}
</style>
