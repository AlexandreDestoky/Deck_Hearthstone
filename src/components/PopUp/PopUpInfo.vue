<template>
  <div class="popUp" :class="{ invisible: popUpInvisible }" @click="dissimulePopUp">
    <div class="textBox">
      <span class="croix">❌</span>
      <div class="contenu">
        <h3>{{titre}}</h3>
        <p>{{texte}}</p>
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
      texte: ""
    };
  },
    created() {
    /**
     * Réception de l'évenement "popUpInfoVisible"
     * Affiche le popUp en lui donnant les infos dont il a besoin
     */
    bus.$on("popUpInfoVisible", (data)=> {
      //On attribue les données reçue au titre et texte du popUp
      this.titre = data[0];
      this.texte = data[1];
      // On rend le popUp visible
      this.popUpInvisible = false;
    })
  },
  methods: {
    /**
     * Fonction de dissimulation du popUp
     */
    dissimulePopUp(e) {
      // Le popUp disparait si on clique sur la croix rouge ou en dehors du popUp
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
    font-size: 1.6rem;
    cursor: pointer;
  }

  .invisible {
    display: none;
  }
</style>
