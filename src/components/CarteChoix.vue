<template>
  <!--------------------------- CARTE --------------------------->
  <div class="carteChoix col-12 col-md-8">
    <p>{{ triChoisi }} salut</p>
    <div class="choixClasseCarte">
      <div class="classeDeck">{{ classeChoisie === undefined ? "CLASS" : formatageClasse(classeChoisie) }} CARDS</div>
      <div class="neutre">NEUTRAL CARDS</div>
    </div>
    <p>salut : {{tabCarte.length}}</p>
    <div class="carteAffichage">
      <div class="carte" v-for="tabObj of tabCarte" :key="tabObj.cardId">
          <img :src="tabObj.img" alt=""/>
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
      tabCarte: undefined,
      classeChoisie: undefined,
      triChoisi: undefined,
    };
  },
  created() {
    //On donne à tabCarte la liste des cartes standard
    this.tabCarte = this.fetchTest("NEUTRAL");
    //On donne à classeChoisie la classe choisie dans le composant choixPerso
    bus.$on("choixClasse", (data) => {
      this.classeChoisie = data;
    }),
      bus.$on("choixTri", (data) => {
        this.triChoisi = data;
      });
  },
  methods: {
    fetchTest(classe) {
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
          // let core = response["Core"];
          // core = core.filter((x) => x["collectible"] === true);
          // let ash = response["Ashes of Outland"];
          // ash = ash.filter((x) => x["collectible"] === true);
          // let sch = response["Scholomance Academy"];
          // sch = sch.filter((x) => x["collectible"] === true);
          // let mad = response["Madness At The Darkmoon Faire"];
          // mad = mad.filter((x) => x["collectible"] === true);
          // let forg = response["Forged in the Barrens"];
          // forg = forg.filter((x) => x["collectible"] === true);
          // tabTest.push(core, ash, sch, mad, forg);
          for (let el of response) {
            if (el.cardSet === "Core" || el.cardSet === "Forged in the Barrens") {
              tabTest.push(el);
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
