<template>
  <!--------------------------- BARRE DE TRI -------------------------  -->
  <div class="barreTri">
    <div class="mana col-12 col-md-3">
      <label for="manaOption">Mana</label>
      <select id="manaOption" name="manaOption" v-model="triCards.mana" @change="envoiTri">
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
      <select id="setOption" name="setOption" v-model="triCards.set" @change="envoiTri">
        <option value="all">All</option>
        <option value="Core">Core Set</option>
        <option value="Ashes">Ashes of Outland</option>
        <option value="Scholomance">Scholomance Academy</option>
        <option value="Darkmoon Faire">Madness At The Darkmoon Faire</option>
        <option value="Barrens">Forged in the Barrens</option>
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
        <option value="beast">Beast</option>
        <option value="demon">Demon</option>
        <option value="dragon">Dragon</option>
        <option value="elemental">Elemental</option>
        <option value="murloc">Murloc</option>
        <option value="mech">Mech</option>
        <option value="totem">Totem</option>
        <option value="pirate">Pirate</option>
      </select>
    </div>
    <div class="rarete col-12 col-md-3">
      <label for="rareteOption">Rarity</label>
      <select id="rareteOption" name="rareteOption" v-model="triCards.rarity" @change="envoiTri">
        <option value="all">All</option>
        <option value="common">Common</option>
        <option value="rare">Rare</option>
        <option value="epic">Epic</option>
        <option value="legendary">Legendary</option>
      </select>
    </div>
    <div class="spellSchool col-12 col-md-3">
      <label for="spellSchoolOption">Spell School</label>
      <select id="mecaniqueOption" name="mecaniqueOption" v-model="triCards.spell_school" @change="envoiTri">
        <option value="all">All</option>
        <option value="arcane">Arcane</option>
        <option value="fire">Fire</option>
        <option value="shadow">Shadow</option>
        <option value="nature">Nature</option>
        <option value="holy">Holy</option>
        <option value="frost">Frost</option>
        <option value="fel">Fel</option>
      </select>
    </div>
    <p>Salut : {{triCards.rarity}}</p>
  </div>
</template>

<script>
import {bus} from "../main"
export default {
  data() {
    return {
      triCards : {
        mana : "all",
        set : "all",
        attack : "all",
        health : "all",
        type:"all",
        race:"all",
        rarity: "all",
        spell_school : "all"
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
