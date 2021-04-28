<template>
  <div class="choixClasse">
    <!---------------------- CLASSE HEARTHSTONE-------------------- -->
    <div class="classeHS" :class="{ active: isActive['shaman'] }" id="shaman" @click="toggleActive">
      <img src="../assets/images/chaman.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['demon_hunter'] }" id="demon_hunter" @click="toggleActive">
      <img src="../assets/images/chasseur-de-demons.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['hunter'] }" id="hunter" @click="toggleActive">
      <img src="../assets/images/chasseur.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['warlock'] }" id="warlock" @click="toggleActive">
      <img src="../assets/images/demoniste.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['druid'] }" id="druid" @click="toggleActive">
      <img src="../assets/images/druide.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['warrior'] }" id="warrior" @click="toggleActive">
      <img src="../assets/images/guerrier.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['mage'] }" id="mage" @click="toggleActive">
      <img src="../assets/images/mage.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['paladin'] }" id="paladin" @click="toggleActive">
      <img src="../assets/images/paladin.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['priest'] }" id="priest" @click="toggleActive">
      <img src="../assets/images/pretre.jpg" alt="" />
    </div>
    <div class="classeHS" :class="{ active: isActive['rogue'] }" id="rogue" @click="toggleActive">
      <img src="../assets/images/voleur.jpg" alt="" />
    </div>
  </div>
</template>

<script>
import {bus} from "../main"
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
      this.choixClasse = classe;
      bus.$emit("choixClasse",classe);
    },
  },
};
</script>

<style scoped></style>
