<template>
  <!--------------------------- CARTE --------------------------->
  <div class="carteChoix col-12 col-md-8">
    <!-- Affichage classes de carte-->
    <div class="choixClasseCarte">
      <div class="classeDeck" @click="affichageClasse = true">
        {{ classeChoisie === undefined ? "CLASS" : formatageClasse(classeChoisie) }} CARDS
      </div>
      <div class="neutre" @click="affichageClasse = false">NEUTRAL CARDS</div>
    </div>
    <div class="carteAffichage" :class="{ couleurClasse: affichageClasse, couleurNeutre: !affichageClasse }">
      <h1 v-if="classeChoisie === undefined && affichageClasse">Veuillez choisir une classe</h1>
      <div class="boxCard">
        <div
          class="carte"
          v-for="tabObj of affichageClasse ? tabCarteClasse : tabCarte"
          :key="tabObj.cardId"
          :class="{ indisponible: cartesPlusDispo.includes(tabObj.name) }"
        >
          <img
            :src="tabObj.img"
            :alt="tabObj.name"
            @click="envoiCarte(tabObj)"
            draggable="true"
            @dragstart="drag"
            :id="tabObj.cardId"
            :data-carte-nom="tabObj.name"
            :data-carte-cout="tabObj.cost"
          />
        </div>
      </div>
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
      affichageClasse: true,
      couleurClasse: true,
      couleurNeutre: false,
      cartesPlusDispo: [],
    };
  },
  created() {
    //On donne à tabCarte la liste des cartes standard
    this.tabCarte = this.fetchTest("NEUTRAL");
    //On donne à classeChoisie la classe choisie dans le composant choixPerso
    bus.$on("choixClasse", (data) => {
      this.classeChoisie = data;
      this.tabCarteClasse = this.fetchTest(data);
    });
    bus.$on("choixTri", (data) => {
      this.triChoisi = data;
      this.tabCarte = this.fetchTest("NEUTRAL", this.triChoisi);
      if (this.classeChoisie) this.tabCarteClasse = this.fetchTest(this.classeChoisie, this.triChoisi);
    });
    bus.$on("cartePlusDispo", (data) => {
      this.cartesPlusDispo.push(data);
      console.log(this.cartesPlusDispo);
    });
  },
  methods: {
    fetchTest(classe, tris) {
      let tabTest = []; //le tableau qui va contenir toutes les cartes
      fetch(`https://omgvamp-hearthstone-v1.p.rapidapi.com/cards/classes/${classe}?collectible=1`, {
        method: "GET",
        headers: {
          "x-rapidapi-key": "c09d1f1348msh9846d0798360c9bp131a6bjsn1ec4841db391",
          "x-rapidapi-host": "omgvamp-hearthstone-v1.p.rapidapi.com",
        },
      })
        .then((reponse) => reponse.json())
        .then((response) => {
          for (let carte of response) {
            let toutCritere = true; //de base tout les critères sont remplis

            //On test si la carte est dans un des formats du standard
            if (
              carte.cardSet === "Core" ||
              carte.cardSet === "Ashes of Outland" ||
              carte.cardSet === "Scholomance Academy" ||
              carte.cardSet === "Madness At The Darkmoon Faire" ||
              carte.cardSet === "Forged in the Barrens"
            ) {
              //Si il y a un tri
              if (tris != undefined) {
                //Pour chaque tri (attaque,cout..)
                for (let tri in tris) {
                  //Si le tri est différent de all (car si c'est all il n'y a pas de tri qui s'applique)
                  if (tris[tri] != "all") {
                    //SI la valeur du tri est différente de la valeur de la carte, tout les critère ne sont pas rempli
                    if (carte[tri] != tris[tri]) toutCritere = false;
                  }
                }
                //SSI tout les critères sont OK, on ajoute la carte au tableau
                if (toutCritere === true) tabTest.push(carte);
                //Si il n'y a pas de tri, on ajoute toutes les cartes standard
              } else {
                tabTest.push(carte);
              }
            }
          }
        });
      return tabTest;
    },
    formatageClasse(classe) {
      return classe.replace("_", " ").toUpperCase();
    },
    envoiCarte(carte) {
      bus.$emit("choixCarte", carte);
    },
    drag(ev) {
      ev.dataTransfer.setData("nom", ev.target.dataset.carteNom);
      ev.dataTransfer.setData("cout", ev.target.dataset.carteCout);
    },
  },
};
</script>

<style scoped></style>
