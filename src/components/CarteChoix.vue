<template>
  <!--------------------------- CARTE --------------------------->
  <div class="carteChoix col-12 col-md-8">
    
    <!-- Partie Choix Carte Classe / Neutre-->
    <div class="choixClasseCarte">
      <div class="classeDeck" @click="affichageClasse = true">
        {{ classeChoisie === undefined ? "CLASS" : formatageClasse(classeChoisie) }} CARDS
      </div>
      <div class="neutre" @click="affichageClasse = false">NEUTRAL CARDS</div>
    </div>

    <!-- Partie Affichage des Cartes -->
    <div class="carteAffichage" :class="{ couleurClasse: affichageClasse, couleurNeutre: !affichageClasse }">
      <h1 v-if="classeChoisie === undefined" class="info-choix-classe">Please choose a class</h1>
      <div class="boxCard" v-else>
        <div
          class="carte"
          v-for="carte of affichageClasse ? tabCarteClasse : tabCarteNeutre"
          :key="carte.cardId"
          :class="{ indisponible: cartesPlusDispo.includes(carte.name) }"
        >
          <img
            :src="carte.img"
            :alt="carte.name"
            @click="envoiCarte(carte)"
            draggable="true"
            @dragstart="drag"
            :id="carte.cardId"
            :data-carte-nom="carte.name"
            :data-carte-cout="carte.cost"
            :data-carte-rarete="carte.rarity"
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
      tabCarteNeutre: undefined, // Les cartes neutres
      tabCarteClasse: undefined, // Les cartes de la classe choisie
      affichageClasse: true, // Par défaut on affichage les cartes de classes
      cartesPlusDispo: [],
    };
  },
  created() {
    //On attribue les cartes neutres dés le chargement de la page
    this.tabCarteNeutre = this.requeteApi("NEUTRAL");
    /**
     * Réception de l'évenement "choixClasse"
     * Actualisation de la classe choisie et des cartes de classes affichées
     */
    bus.$on("choixClasse", (data) => {
      this.classeChoisie = data;
      this.tabCarteClasse = this.requeteApi(data);
    });
    /**
     * Réception de l'évenement "choixTri"
     * On effectue le tri sur les cartes Neutres
     * On effectue le tri sur les cartes de classes si il y a une classe choisie
     */
    bus.$on("choixTri", (data) => {
      this.triChoisi = data;
      this.tabCarteNeutre = this.requeteApi("NEUTRAL", this.triChoisi);
      if (this.classeChoisie) this.tabCarteClasse = this.requeteApi(this.classeChoisie, this.triChoisi);
    });
    /**
     * Réception de l'évenement "cartePlusDispo"
     * On ajoute la carte reçue de la liste de carte plus dispo
     */
    bus.$on("cartePlusDispo", (data) => {
      this.cartesPlusDispo.push(data);
      localStorage.setItem("plusDispo", JSON.stringify(this.cartesPlusDispo)); //LocalStorage => on actualise la liste de carte plus dispo
    });
    /**
     * Réception de l'évenement "carteReDispo" 
     * On enleve la carte reçue de la liste de carte plus dispo
     */
    bus.$on("carteReDispo", (data) => {
      let index = this.cartesPlusDispo.indexOf(data);
      this.cartesPlusDispo.splice(index, 1);
      localStorage.setItem("plusDispo", JSON.stringify(this.cartesPlusDispo)); //LocalStorage => on actualise la liste de carte plus dispo
    });
    /**
     * Réception de l'évenement "vidagePlusDispo" suite a validation du popUpChoice
     * Si true on vide la liste de carte plus dispo
     */
    bus.$on("vidagePlusDispo", (data) => {
      if (data) {
        this.cartesPlusDispo = [];
        localStorage.setItem("plusDispo", JSON.stringify(this.cartesPlusDispo)); //LocalStorage => on actualise la liste de carte plus dispo
      }
    });
  },
  mounted() {
    /**
     * Si il y a un localStorage pour les cartes plusDispo
     * On attribue la valeur à la liste de cartes plus dispo
     */
    if (localStorage.getItem("plusDispo")) {
      try {
        this.cartesPlusDispo = JSON.parse(localStorage.getItem("plusDispo"));
      } catch (e) {
        localStorage.removeItem("plusDispo");
      }
    }
  },
  methods: {
    /**
     * Fonction de requète API
     * @classe : la classe pour laquel effectué la requête
     * @tris : les critères de tri à effectué 
     * @return : Renvoi un tableau de cartes correspondante à la classe et au tri demandé
     */
    requeteApi(classe, tris) {
      let tabFetch = []; //le tableau qui va contenir toutes les cartes
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
                if (toutCritere === true) tabFetch.push(carte);
              //Si il n'y a pas de tri, on ajoute toutes les cartes standard
              } else {
                tabFetch.push(carte);
              }
            }
          }
        });
      return tabFetch;
    },
    /**
     * Fonction de formatage de la classe reçue
     * @return la classe à laquelle on a enlevé les underscore et mise en majuscule
     */
    formatageClasse(classe) {
      return classe.replace("_", " ").toUpperCase();
    },
    /**
     * Envoi de l'évenement "choixCarte"
     * On envoi la carte sur laquel on clique au deck
     */
    envoiCarte(carte) {
      bus.$emit("choixCarte", carte);
    },
    /**
     * Fonction d'envoi de données quand on Drag une carte 
     */
    drag(ev) {
      //Données viennent du dataSet
      ev.dataTransfer.setData("nom", ev.target.dataset.carteNom);
      ev.dataTransfer.setData("cout", ev.target.dataset.carteCout);
      ev.dataTransfer.setData("rarete", ev.target.dataset.carteRarete);
    },
  },
};
</script>

<style scoped></style>
