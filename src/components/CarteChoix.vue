<template>
  <!--------------------------- CARTE --------------------------->
  <div class="carteChoix col-12 col-md-8">
    <p>{{triChoisi}} salut</p>
    <div class="choixClasseCarte">
      <div class="classeDeck">{{classeChoisie===undefined?"CLASS":formatageClasse(classeChoisie)}} CARDS</div>
      <div class="neutre">NEUTRAL CARDS</div>
    </div>
    <div class="carteAffichage">
      <template v-for="tabObj of tabCarte">
        <div class="carte" :key="ob.cardId" v-for="ob of tabObj">
          <img :src="ob.img" alt="" />
          <!-- <p>{{ob}}</p> -->
        </div>
      </template>
      <!-- <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte indisponible"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>

      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div> -->
    </div>
  </div>
</template>

<script>
import {bus} from "../main"
export default {
  data() {
    return {
      tabCarte: undefined,
      classeChoisie : undefined,
      triChoisi : undefined
    };
  },
  created() {
    //On donne à tabCarte la liste des cartes standard
    this.tabCarte = this.fetchTest();
    //On donne à classeChoisie la classe choisie dans le composant choixPerso
    bus.$on("choixClasse",(data) => {
      this.classeChoisie = data;
    }),
    bus.$on("choixTri",(data)=> {
      this.triChoisi = data
    })

  },
  methods: {
    fetchTest() {
      let tabTest = [];
      fetch("https://omgvamp-hearthstone-v1.p.rapidapi.com/cards", {
        method: "GET",
        headers: {
          "x-rapidapi-key": "c09d1f1348msh9846d0798360c9bp131a6bjsn1ec4841db391",
          "x-rapidapi-host": "omgvamp-hearthstone-v1.p.rapidapi.com",
        },
      })
        .then((reponse) => reponse.json())
        .then((response) => {
          let core = response["Core"];
          core = core.filter((x) => x["collectible"] === true);
          let ash = response["Ashes of Outland"];
          ash = ash.filter((x) => x["collectible"] === true);
          let sch = response["Scholomance Academy"];
          sch = sch.filter((x) => x["collectible"] === true);
          let mad = response["Madness At The Darkmoon Faire"];
          mad = mad.filter((x) => x["collectible"] === true);
          let forg = response["Forged in the Barrens"];
          forg = forg.filter((x) => x["collectible"] === true);
          tabTest.push(core, ash, sch, mad, forg);
        });
      return tabTest;
    },
    formatageClasse(classe) {
      return classe.replace("_"," ").toUpperCase();
    }
  },
};
</script>

<style scoped></style>
