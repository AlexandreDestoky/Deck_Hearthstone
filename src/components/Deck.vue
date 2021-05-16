<template>
  <!-------------------------------- DECK -------------------------------->
  <div class="deck col-12 col-md-4">
    <div class="classeCard">
      <p>DECK CHAMAN</p>
      <p>0/30</p>
    </div>
    <div class="deckListe" @drop="drop" @dragover="allowDrop">
      <div class="carteListe" v-for="el of deckList" :key="el.name">
        <p class="coutMana">{{ el.cost }}</p>
        <p class="nom">{{ el.name }}</p>
        <p class="nbrExemplaire">X1</p>
      </div>
        <h1>coucou</h1>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      deckList: [],
    };
  },
  created() {
    bus.$on("choixCarte", (data) => {
      this.deckList.push(data);
    });
  },
  methods: {
    allowDrop(ev) {
      ev.preventDefault();
    },
    drop(ev) {
      ev.preventDefault();
      let data = ev.dataTransfer.getData("text");
      console.log(data);
      // var nodeCopy = document.getElementById(data).cloneNode(true);
      // nodeCopy.id = "newId";
      // console.log(nodeCopy);
      // ev.target.appendChild(nodeCopy);
    },
  },
};
</script>

<style scoped></style>
