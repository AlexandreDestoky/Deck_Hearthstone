<template>
  <!-- MODALE -->
  <div class="popUp" :class="{ invisible: popUpInvisible }" @click="removePopUp">
    <div class="textBox">
      <span class="croix">❌</span>
      <div class="contenu">
        <!-- <h3>Votre deck est plein !</h3>
        <p>Vous devez d'abord retirer une carte avant d'en ajouter une.</p> -->
        <h3>{{titre}}</h3>
        <p>{{texte}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      popUpInvisible: true,
      titre: "",
      texte: ""
    };
  },
    created() {
    bus.$on("popUpInvisible", (data)=> {
      this.titre = data[0];
      this.texte = data[1];
      this.popUpInvisible = false;
    })
  },
  methods: {
    removePopUp(e) {
      if (e.target.classList.contains("popUp") || e.target.classList.contains("croix")) {
        this.popUpInvisible = true;
      }
    },
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
  padding: 5%;
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
</style>
