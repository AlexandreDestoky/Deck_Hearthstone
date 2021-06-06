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
    /**
     * Réception de l'évenement "nbrCartes"
     * Sert à savoir si le deck est vide ou non
     */
    bus.$on("nbrCartes", (data) => {
      this.deckVide = data; 
    });
    /**
     * Réception de l'évenement "changementClasse"
     * (Lorsque l'on valide le popUpChoice)
     */
    bus.$on("changementClasse", (data) => {
      this.deckVide = true;
      this.classChange(data);
      localStorage.setItem("classe",data);
    });
  },
  mounted() {
    /**
     * Si il y a un localStorage pour la classe
     * On effectue les changements de classe
     */
    if (localStorage.getItem("classe")) {
      let classe = localStorage.getItem("classe");
      this.classChange(classe);
      this.envoiChoixClasse(classe);
    }
  },
  methods: {
    /**
     * Fonction de test si le deck est vide ou non lorsque l'on change de classe
     */
    toggleActive(e) {
      let nouvelleClasse = e.target.parentNode.id;
      if (this.deckVide) {
        this.classChange(nouvelleClasse);
        localStorage.setItem("classe",nouvelleClasse); //LocalStorage => le nom de la classe change
      } else { 
        this.openPopUpChoice(nouvelleClasse); // si le deck n'est pas vide, on envoi le nom de la classe au popup
      }
    },
    /**
     * Fonction de changement de classe
     */
    classChange(nouvelleClasse) {
      //Change le visuel pour que l'on voit la nouvelle classe (bordure verte)
      for (let el in this.isActive) this.isActive[el] = false;
      this.isActive[nouvelleClasse] = true;
      // Lance la fonction qui émet l'évenement choixClasse 
      this.envoiChoixClasse(nouvelleClasse);
    },
    /**
     * Envoi de l'évenement "choixClasse"
     * (Les autres composants auront accès au nom de la nouvelle classe)
     */
    envoiChoixClasse(nomClasse) {
      bus.$emit("choixClasse", nomClasse);
    },
    /**
     * Envoi de l'évenement "popUpChoiceVisible"
     * On envoi le titre et texte que l'on veut dans le popUp et le nom de la nouvelle classe choisie
     */
    openPopUpChoice(nouvelleClasse) {
      bus.$emit("popUpChoiceVisible", [this.popUpChoiceTitre, this.popUpChoiceTexte, nouvelleClasse]);
    },

  },
};
</script>

<style scoped></style>
