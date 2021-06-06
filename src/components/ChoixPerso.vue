<template>
  <div class="choixClasse">
    <!---------------------- CLASSE HEARTHSTONE-------------------- -->
    <div class="classeHS" :class="{ active: isActive['shaman'],incliquable:isActive['shaman'] }" id="shaman" @click="toggleActive">
      <img src="../assets/images/chaman.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['demon_hunter'],incliquable:isActive['demon_hunter'] }" id="demon_hunter" @click="toggleActive">
      <img src="../assets/images/chasseur-de-demons.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['hunter'],incliquable:isActive['hunter'] }" id="hunter" @click="toggleActive">
      <img src="../assets/images/chasseur.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['warlock'],incliquable:isActive['warlock'] }" id="warlock" @click="toggleActive">
      <img src="../assets/images/demoniste.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['druid'],incliquable:isActive['druid'] }" id="druid" @click="toggleActive">
      <img src="../assets/images/druide.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['warrior'],incliquable:isActive['warrior'] }" id="warrior" @click="toggleActive">
      <img src="../assets/images/guerrier.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['mage'],incliquable:isActive['mage'] }" id="mage" @click="toggleActive">
      <img src="../assets/images/mage.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['paladin'],incliquable:isActive['paladin'] }" id="paladin" @click="toggleActive">
      <img src="../assets/images/paladin.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['priest'],incliquable:isActive['priest'] }" id="priest" @click="toggleActive">
      <img src="../assets/images/pretre.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['rogue'],incliquable:isActive['rogue'] }" id="rogue" @click="toggleActive">
      <img src="../assets/images/voleur.jpg" alt="" />
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      isActive: {
        shaman: false,
        hunter: false,
        demon_hunter: false,
        warlock: false,
        druid: false,
        warrior: false,
        mage: false,
        paladin: false,
        priest: false,
        rogue: false,
      },
      deckVide: true,
      popUpChoiceTitre: "Vous avez déjà un deck pour une autre classe !",
      popUpChoiceTexte: "Si vous changer de classe, votre deck actuel sera supprimé",
    };
  },
  created() {
    //Le nombre de cartes reçu par le deck 
    bus.$on("nbrCartes", (data) => {
      this.deckVide = data;
    });
    bus.$on("changementClasse", (data) => {
      this.deckVide = true;
      this.classChange(data);
      localStorage.setItem("classe",data);
    });
  },
  mounted() {
    if (localStorage.getItem("classe")) {
      let classe = localStorage.getItem("classe");
      this.classChange(classe);
      this.envoiChoixClasse(classe);
    }
  },
  methods: {
    /**
     * Met l'image sélectionné en active et enlève le active des autres images
     */
    toggleActive(e) {
      let nouvelleClasse = e.target.parentNode.id; // QUand on clique sur une classe, elle se met en classeCHoisie
      if (this.deckVide) { // Si le deck est vide, on change le visuel de la classe choisie
        this.classChange(nouvelleClasse);
        localStorage.setItem("classe",nouvelleClasse); // Changement localStorage
      } else { // SINON ON OUvre le popUp avec la nouvelle classe
        this.openPopUpChoice(nouvelleClasse); // si le deck n'est pas vide, on envoi le nom de la classe au popup
      }
    },
    /**
     * On envoi le nom de la classe
     */
    envoiChoixClasse(nomClasse) {
      bus.$emit("choixClasse", nomClasse);
    },
    /**
     * On ouvre le pop up de CHOIX
     */
    openPopUpChoice(nouvelleClasse) {
      bus.$emit("popUpChoiceVisible", [this.popUpChoiceTitre, this.popUpChoiceTexte, nouvelleClasse]);
    },
    /**
     * Changment de classe
     */
    classChange(nouvelleClasse) {
      for (let el in this.isActive) {
        this.isActive[el] = false;
      }
      this.isActive[nouvelleClasse] = true; // on met la classe choisie en true
      this.choixClasse = nouvelleClasse;
      this.envoiChoixClasse(nouvelleClasse);
    },
  },
};
</script>

<style scoped></style>
