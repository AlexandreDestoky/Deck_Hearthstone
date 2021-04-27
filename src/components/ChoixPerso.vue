<template>
  <div class="choixClasse">
    <!---------------------- CLASSE HEARTHSTONE-------------------- -->
    <div class="classeHS" :class="{ active: isActive['chaman'] }" id="chaman" @click="toggleActive">
      <img src="../assets/images/chaman.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['dh'] }" id="dh" @click="toggleActive">
      <img src="../assets/images/chasseur-de-demons.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['chasseur'] }" id="chasseur" @click="toggleActive">
      <img src="../assets/images/chasseur.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['demoniste'] }" id="demoniste" @click="toggleActive">
      <img src="../assets/images/demoniste.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['druide'] }" id="druide" @click="toggleActive">
      <img src="../assets/images/druide.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['guerrier'] }" id="guerrier" @click="toggleActive">
      <img src="../assets/images/guerrier.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['mage'] }" id="mage" @click="toggleActive">
      <img src="../assets/images/mage.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['paladin'] }" id="paladin" @click="toggleActive">
      <img src="../assets/images/paladin.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['pretre'] }" id="pretre" @click="toggleActive">
      <img src="../assets/images/pretre.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['voleur'] }" id="voleur" @click="toggleActive">
      <img src="../assets/images/voleur.jpg" alt="" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: {
        chaman: false,
        chasseur: false,
        dh: false,
        demoniste: false,
        druide: false,
        guerrier: false,
        mage: false,
        paladin: false,
        pretre: false,
        voleur: false,
      },
    };
  },
  methods: {
    /**
     * Met l'image sélectionné en active et enlève le active des autres images
     */
    toggleActive(e) {
      let classe = e.target.parentNode.id;
      for(let el in this.isActive) {
        this.isActive[el] = false;
      }
      this.isActive[classe] = true;
    },
  },
};
</script>

<style scoped></style>
