<template>
  <!-- MODALE -->
  <div class="popUp" :class="{ invisible: popUpInvisible }" @click="removePopUp">
    <div class="textBox">
      <div class="contenu">
        <h3>Vous avez déjà un deck pour une autre classe !</h3>
        <p>Si vous changer de classe, votre deck actuel sera supprimé</p>
      </div>
      <div class="boutons">
        <a href="#" class="validate" @click="viderDeck">OK</a>
        <a href="#" class="cancel">ANNULER</a>
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
    bus.$on("popUpChoiceVisible", (data) => {
      this.titre = data[0];
      this.texte = data[1];
      this.nomNouvelleClasse = data[2];
      this.popUpInvisible = false;
    });
  },
  methods: {
    removePopUp(e) {
      if (e.target.classList.contains("popUp") || e.target.classList.contains("cancel")) {
        this.popUpInvisible = true;
      }
    },
    viderDeck() {
      bus.$emit("vidageDeck",true);
      bus.$emit("changementClasse",this.nomNouvelleClasse); // on informe ChoixPerso que l'on veut changer de classe
      bus.$emit("vidagePlusDispo", true);
      this.popUpInvisible = true;
      localStorage.removeItem("deckListe");
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

.boutons a {
  padding: 0.2em 1em;
  border: 2px solid black;
  border-radius: 20px;
  margin: 0 0.5em;
  width: 40%;
  text-align: center;
  text-decoration: none;
  color: #eee;
}

.validate {
  background-color: green;
}

.validate:hover {
  background-color: #0b380b;
  box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.336);
}

.boutons a:hover {
  box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.336);
}
.cancel {
  background-color: firebrick;
  display: table;
}
.cancel:hover {
  background-color: #6f0a0a;
}
</style>
