<template>
     <!--------------------------- BARRE DE TRI -------------------------  -->
      <div class="barreTri">
        <div class="mana col-12 col-md-3">
          <label for="manaOption">Mana</label>
          <select id="manaOption" name="manaOption">
            <option value="all">Toutes</option>
            <option value="0">Mana: 0</option>
            <option value="1">Mana: 1</option>
            <option value="2">Mana: 2</option>
            <option value="3">Mana: 3</option>
          </select>
        </div>
        <div class="extension col-12 col-md-3">
          <label for="extensionOption">Extension</label>
          <select id="extensionOption" name="extensionOption">
            <option value="all">Toutes</option>
            <option value="barrens">Barrens</option>
            <option value="Scholomance">Scholomance</option>
            <option value="Sombrelune">Sombrelune</option>
            <option value="Fondamental">Fondamental</option>
          </select>
        </div>
        <div class="attaque col-12 col-md-3">
          <label for="attaqueOption">Attaque</label>
          <select id="attaqueOption" name="attaqueOption">
            <option value="all">Toutes</option>
            <option value="0">Attaque: 0</option>
            <option value="1">Attaque: 1</option>
            <option value="2">Attaque: 2</option>
            <option value="3">Attaque: 3</option>
          </select>
        </div>
        <div class="vie col-12 col-md-3">
          <label for="vieOption">Vie</label>
          <select id="vieOption" name="vieOption">
            <option value="all">Toutes</option>
            <option value="0">Vie: 0</option>
            <option value="1">Vie: 1</option>
            <option value="2">Vie: 2</option>
            <option value="3">Vie: 3</option>
          </select>
        </div>
        <div class="typeCarte col-12 col-md-3">
          <label for="typeCarteOption">Type</label>
          <select id="typeCarteOption" name="typeCarteOption">
            <option value="all">Tous</option>
            <option value="Heros">Héros</option>
            <option value="Serviteur">Serviteur</option>
            <option value="Sort">Sort</option>
            <option value="Arme">Arme</option>
          </select>
        </div>
        <div class="race col-12 col-md-3">
          <label for="raceOption">Race</label>
          <select id="raceOption" name="raceOption">
            <option value="all">Toutes</option>
            <option value="bete">Bête</option>
            <option value="demon">Demon</option>
            <option value="dragon">Dragon</option>
            <option value="elementaire">Elementaire</option>
          </select>
        </div>
        <div class="rarete col-12 col-md-3">
          <label for="rareteOption">Rareté</label>
          <select id="rareteOption" name="rareteOption">
            <option value="all">Toutes</option>
            <option value="commune">Commune</option>
            <option value="rare">Rare</option>
            <option value="epique">Epique</option>
            <option value="legendaire">Legendaire</option>
          </select>
        </div>
        <div class="mecanique col-12 col-md-3">
          <label for="mecaniqueOption">Mécanique</label>
          <select id="mecaniqueOption" name="mecaniqueOption">
            <option value="all">Toutes</option>
            <option value="0">Attaque: 0</option>
            <option value="1">Attaque: 1</option>
            <option value="2">Attaque: 2</option>
            <option value="3">Attaque: 3</option>
          </select>
        </div>
      </div>
</template>

<script>
export default {
  
}
</script>

<style scoped>

</style>