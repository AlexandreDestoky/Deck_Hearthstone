<template>
  <!--------------------------- CARTE --------------------------->
  <div class="carteChoix col-12 col-md-8">
    <p>{{ triChoisi }} salut</p>
    <div class="choixClasseCarte">
      <div class="classeDeck">{{ classeChoisie === undefined ? "CLASS" : formatageClasse(classeChoisie) }} CARDS</div>
      <div class="neutre">NEUTRAL CARDS</div>
    </div>
    <div class="carteAffichage">
      <div class="carte" v-for="tabObj of tabCarte" :key="tabObj.cardId">
        <img :src="tabObj.img" :alt="tabObj.name" />
      </div>
      <!-- <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div> -->
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
export default {
  data() {
    return {
      classeChoisie: undefined,
      triChoisi: undefined,
      tabCarte: undefined,
      tabCarteClasse: undefined,
    };
  },
  created() {
    //On donne à tabCarte la liste des cartes standard
    this.tabCarte = this.fetchTest("NEUTRAL");
    //On donne à classeChoisie la classe choisie dans le composant choixPerso
    bus.$on("choixClasse", (data) => {
      this.classeChoisie = data;
    });
    bus.$on("choixTri", (data) => {
      this.triChoisi = data;
      this.tabCarte = this.fetchTest("NEUTRAL",this.triChoisi);
    });
  },
  methods: {
    fetchTest(classe, tris) {
      let tabTest = [];
      fetch(`https://omgvamp-hearthstone-v1.p.rapidapi.com/cards/classes/${classe}?collectible=1`, {
        method: "GET",
        headers: {
          "x-rapidapi-key": "c09d1f1348msh9846d0798360c9bp131a6bjsn1ec4841db391",
          "x-rapidapi-host": "omgvamp-hearthstone-v1.p.rapidapi.com",
        },
      })
        .then((reponse) => reponse.json())
        .then((response) => {
          for (let el of response) {
            //On test si la carte est dans un des formats du standard
            if (
              // el.cardSet === "Core" ||
              // el.cardSet === "Ashes of Outland" ||
              // el.cardSet === "Scholomance Academy" ||
              // el.cardSet === "Madness At The Darkmoon Faire" ||
              el.cardSet === "Forged in the Barrens"
            ) {
              tabTest.push(el);
              //Si le tri existe
              if(tris !== undefined) {
                for(let tri in tris) {

                  if(el[tri] !== undefined) {
                    console.log(el[tri] + " " + tri);
                    console.log(tris[tri]);
                    console.log("-----------------");
                  }
                }
              }
            }
          }
        });
      return tabTest;
    },
    formatageClasse(classe) {
      return classe.replace("_", " ").toUpperCase();
    },
  },
};
</script>

<style scoped></style>
