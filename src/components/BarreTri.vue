<template>
  <!--------------------------- BARRE DE TRI -------------------------  -->
  <div class="barreTri">
    <div class="mana col-12 col-md-3">
      <label for="manaOption">Mana</label>
      <select id="manaOption" name="manaOption" v-model="triCartes.cost" @change="envoiTri">
        <option value="all">All</option>
        <template v-for="el in 13">
          <option :value="el-1" :key="el">Mana : {{el-1}}</option>
        </template>
      </select>
    </div>
    <div class="set col-12 col-md-3">
      <label for="setOption">Set</label>
      <select id="setOption" name="setOption" v-model="triCartes.cardSet" @change="envoiTri">
        <option value="all">All</option>
        <option value="Core">Core Set</option>
        <option value="Ashes of Outland">Ashes of Outland</option>
        <option value="Scholomance Academy">Scholomance Academy</option>
        <option value="Madness At The Darkmoon Faire">Madness At The Darkmoon Faire</option>
        <option value="Forged in the Barrens">Forged in the Barrens</option>
      </select>
    </div>
    <div class="attaque col-12 col-md-3">
      <label for="attaqueOption">Attack</label>
      <select id="attaqueOption" name="attaqueOption" v-model="triCartes.attack" @change="envoiTri">
        <option value="all">All</option>
        <template v-for="el in 13">
          <option :value="el-1" :key="el">Attaque : {{el-1}}</option>
        </template>
      </select>
    </div>
    <div class="vie col-12 col-md-3">
      <label for="vieOption">Health</label>
      <select id="vieOption" name="vieOption" v-model="triCartes.health" @change="envoiTri">
        <option value="all">All</option>
        <template v-for="el in 13">
          <option :value="el-1" :key="el">Health : {{el-1}}</option>
        </template>
      </select>
    </div>
    <div class="typeCarte col-12 col-md-3">
      <label for="typeCarteOption">Type</label>
      <select id="typeCarteOption" name="typeCarteOption" v-model="triCartes.type" @change="envoiTri">
        <option value="all">All</option>
        <option value="Heros">Heros</option>
        <option value="Minion">Minion</option>
        <option value="Spell">Spell</option>
        <option value="Weapon">Weapon</option>
      </select>
    </div>
    <div class="race col-12 col-md-3">
      <label for="raceOption">Race</label>
      <select id="raceOption" name="raceOption" v-model="triCartes.race" @change="envoiTri">
        <option value="all">All</option>
        <option value="Beast">Beast</option>
        <option value="Demon">Demon</option>
        <option value="Dragon">Dragon</option>
        <option value="Elemental">Elemental</option>
        <option value="Murloc">Murloc</option>
        <option value="Mech">Mech</option>
        <option value="Totem">Totem</option>
        <option value="Pirate">Pirate</option>
      </select>
    </div>
    <div class="rarete col-12 col-md-3">
      <label for="rareteOption">Rarity</label>
      <select id="rareteOption" name="rareteOption" v-model="triCartes.rarity" @change="envoiTri">
        <option value="all">All</option>
        <option value="Common">Common</option>
        <option value="Rare">Rare</option>
        <option value="Epic">Epic</option>
        <option value="Legendary">Legendary</option>
      </select>
    </div>
    <div class="spellSchool col-12 col-md-3">
      <label for="spellSchoolOption">Spell School</label>
      <select id="mecaniqueOption" name="mecaniqueOption" v-model="triCartes.spellSchool" @change="envoiTri">
        <option value="all">All</option>
        <option value="Arcane">Arcane</option>
        <option value="Fire">Fire</option>
        <option value="Shadow">Shadow</option>
        <option value="Nature">Nature</option>
        <option value="Holy">Holy</option>
        <option value="Frost">Frost</option>
        <option value="Fel">Fel</option>
      </select>
    </div>
  </div>
</template>

<script>
import {bus} from "../main"
export default {
  data() {
    return {
      triCartes : {
        cost : "all",
        cardSet : "all",
        attack : "all",
        health : "all",
        type:"all",
        race:"all",
        rarity: "all",
        spellSchool : "all"
      }
    }
  },
  methods: {
    /**
     * Envoi de l'évenement "choixtri" 
     * les données envoyés sont les différents tri
     */
    envoiTri() {
      bus.$emit("choixTri",this.triCartes); 
    }
  },
};
</script>

<style scoped></style>
