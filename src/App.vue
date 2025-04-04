<template>
  <div>
    <div class="navbar">
      <h1>Galerie d'images</h1>

      <input
        type="text"
        v-model="searchQuery"
        placeholder="Rechercher une image..."
      />

      <div class="all-categories">
        <button class="button-categ" @click="currentGallery = 'gallery1'">Galerie 1</button>
        <button class="button-categ" @click="currentGallery = 'gallery2'">Galerie 2</button>
      </div>
    </div>



    <ImageGallery1 v-if="currentGallery === 'gallery1'" :searchQuery="searchQuery" />
    <ImageGallery2 v-if="currentGallery === 'gallery2'" :searchQuery="searchQuery" />
  </div>
</template>

<script>
import ImageGallery1 from "./components/ImageGallery1.vue";
import ImageGallery2 from "./components/ImageGallery2.vue";

export default {
  components: {
    ImageGallery1,
    ImageGallery2,
  },
  data() {
    return {
      currentGallery: 'gallery1', // Définit la galerie par défaut
      searchQuery: '', // Stocke le texte de recherche
    };
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
}

.navbar {
  padding: 0 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

input {
  padding: 10px;
  margin: 10px 0;
  width: 70%;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.all-categories {
  display: flex;
  gap: 10px;
}

.button-categ {
  padding: 10px 20px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

</style>
