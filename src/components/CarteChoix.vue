<template>
  <!--------------------------- CARTE --------------------------->
  <div class="carteChoix col-12 col-md-8">
    <div class="choixClasseCarte">
      <div class="classeDeck">CARTES CHAMAN</div>
      <div class="neutre">CARTES NEUTRES</div>
    </div>
    <div class="carteAffichage">
      <template v-for="(el,index) of tabCarte">
        <div class="carte" :key="index">
          <img :src="el.img" alt="">
          <!-- <p>{{el}}</p> -->
        </div>
      </template>
      <!-- <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte indisponible"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte indisponible"><img src="../assets/images/barjaqueur.png" alt="" /></div>
      <div class="carte"><img src="../assets/images/barjaqueur.png" alt="" /></div>
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
      fetch("https://omgvamp-hearthstone-v1.p.rapidapi.com/cards/sets/Scholomance%20Academy", {
        method: "GET",
        headers: {
          "x-rapidapi-key": "c09d1f1348msh9846d0798360c9bp131a6bjsn1ec4841db391",
          "x-rapidapi-host": "omgvamp-hearthstone-v1.p.rapidapi.com",
        },
      })
        .then((reponse) => reponse.json())
        .then((response) => {
          for (let i = 0; i < response.length; i++) {
            if (response[i]["collectible"] === true) {
              tabTest.push(response[i]);
            }
          }
        });
      return tabTest;
    },
  },
};
</script>

<style scoped></style>
