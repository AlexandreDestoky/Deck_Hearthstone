<template>
  <!--------------------------- CARTE --------------------------->
  <div class="carteChoix col-12 col-md-8">
    <div class="choixClasseCarte">
      <div class="classeDeck">CARTES CHAMAN</div>
      <div class="neutre">CARTES NEUTRES</div>
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
export default {
  data() {
    return {
      tabCarte: undefined,
    };
  },
  created() {
    this.tabCarte = this.fetchTest();
    console.log(this.tabCarte);
    console.log("salut");
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
  },
};
</script>

<style scoped></style>
