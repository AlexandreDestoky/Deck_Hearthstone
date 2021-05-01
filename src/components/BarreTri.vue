<template>
  <!--------------------------- BARRE DE TRI -------------------------  -->
  <div class="barreTri">
    <div class="mana col-12 col-md-3">
      <label for="manaOption">Mana</label>
      <select id="manaOption" name="manaOption" v-model="triCards.cost" @change="envoiTri">
        <option value="all">All</option>
        <option value="0">Mana: 0</option>
        <option value="1">Mana: 1</option>
        <option value="2">Mana: 2</option>
        <option value="3">Mana: 3</option>
        <option value="4">Mana: 4</option>
        <option value="5">Mana: 5</option>
        <option value="6">Mana: 6</option>
        <option value="7">Mana: 7</option>
        <option value="8">Mana: 8</option>
        <option value="9">Mana: 9</option>
        <option value="10">Mana: 10</option>
        <option value="11">Mana: 11</option>
        <option value="12">Mana: 12</option>
      </select>
    </div>
    <div class="set col-12 col-md-3">
      <label for="setOption">Set</label>
      <select id="setOption" name="setOption" v-model="triCards.cardSet" @change="envoiTri">
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
      <select id="attaqueOption" name="attaqueOption" v-model="triCards.attack" @change="envoiTri">
        <option value="all">All</option>
        <option value="0">Attaque: 0</option>
        <option value="1">Attaque: 1</option>
        <option value="2">Attaque: 2</option>
        <option value="3">Attaque: 3</option>
        <option value="4">Attaque: 4</option>
        <option value="5">Attaque: 5</option>
        <option value="6">Attaque: 6</option>
        <option value="7">Attaque: 7</option>
        <option value="8">Attaque: 8</option>
        <option value="9">Attaque: 9</option>
        <option value="10">Attaque: 10</option>
        <option value="11">Attaque: 11</option>
        <option value="12">Attaque: 12</option>
      </select>
    </div>
    <div class="vie col-12 col-md-3">
      <label for="vieOption">Health</label>
      <select id="vieOption" name="vieOption" v-model="triCards.health" @change="envoiTri">
        <option value="all">All</option>
        <option value="0">Health: 0</option>
        <option value="1">Health: 1</option>
        <option value="2">Health: 2</option>
        <option value="3">Health: 3</option>
        <option value="4">Health: 4</option>
        <option value="5">Health: 5</option>
        <option value="6">Health: 6</option>
        <option value="7">Health: 7</option>
        <option value="8">Health: 8</option>
        <option value="9">Health: 9</option>
        <option value="10">Health: 10</option>
        <option value="11">Health: 11</option>
        <option value="12">Health: 12</option>
      </select>
    </div>
    <div class="typeCarte col-12 col-md-3">
      <label for="typeCarteOption">Type</label>
      <select id="typeCarteOption" name="typeCarteOption" v-model="triCards.type" @change="envoiTri">
        <option value="all">All</option>
        <option value="Heros">Heros</option>
        <option value="Minion">Minion</option>
        <option value="Spell">Spell</option>
        <option value="Weapon">Weapon</option>
      </select>
    </div>
    <div class="race col-12 col-md-3">
      <label for="raceOption">Race</label>
      <select id="raceOption" name="raceOption" v-model="triCards.race" @change="envoiTri">
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
      <select id="rareteOption" name="rareteOption" v-model="triCards.rarity" @change="envoiTri">
        <option value="all">All</option>
        <option value="Common">Common</option>
        <option value="Rare">Rare</option>
        <option value="Epic">Epic</option>
        <option value="Legendary">Legendary</option>
      </select>
    </div>
    <div class="spellSchool col-12 col-md-3">
      <label for="spellSchoolOption">Spell School</label>
      <select id="mecaniqueOption" name="mecaniqueOption" v-model="triCards.spellSchool" @change="envoiTri">
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
      triCards : {
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
    envoiTri() {
      bus.$emit("choixTri",this.triCards);
    }
  },
};
</script>

<style scoped></style>
