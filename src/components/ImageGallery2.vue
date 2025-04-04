<template>
  <div>
    <div class="image-grid-2">
      <div class="card-2" v-for="image in filteredImages" :key="image.name">
        <img
          class="img-card-2"
          :src="image.src"
          :alt="image.name"
          @click="openImageModal(image.src)" />
        <p class="text-card-2" @click="copyToClipboard(image.name)">{{ image.name }}</p>
        <small class="category-card-2">{{ image.category }}</small>
      </div>
    </div>

    <!-- Modal d'image -->
    <div v-if="isModalOpen" class="modal" @click="closeImageModal">
      <img :src="selectedImage" alt="Image agrandie" class="modal-image" />
      <button class="close-btn" @click="closeImageModal">Close</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['searchQuery'], // Reçoit la recherche en prop
  data() {
    return {
      categories: ["Compacts", "Sedans", "SUVs", "Coupes", "Muscles", "Sports Classic", "Sports", "Super", "Motorcycles", "Off-Road", "Industrial", "Utility", "Vans", "Cycles", "Boats", "Helicopters", "Planes", "Service", "Emergency", "Military", "Commercial", "Trains", "Open Wheels"],
      allImages: [],
      isModalOpen: false,
      selectedImage: null,
    };
  },
  computed: {
    filteredImages() {
      return this.allImages.filter(image =>
        image.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    }
  },
  async created() {
    for (const category of this.categories) {
      await this.fetchImages(category);
    }
  },
  methods: {
    async fetchImages(category) {
      try {
        const response = await fetch(`/vehicle/${category}/images.json`);
        if (!response.ok) throw new Error("Fichier introuvable");
        const images = await response.json();

        const formattedImages = images.map(img => ({
          name: img.split('.')[0],
          src: `/vehicle/${category}/${img}`,
          category
        }));

        this.allImages = [...this.allImages, ...formattedImages];
      } catch (error) {
        console.error(`Erreur lors du chargement des images pour ${category}:`, error);
      }
    },

    // Copier le nom de l'image dans le presse-papier
    copyToClipboard(text) {
      navigator.clipboard.writeText(text).catch(err => {
        console.error("Erreur lors de la copie : ", err);
      });
    },

    // Ouvrir l'image en mode plein écran
    openImageModal(imageSrc) {
      this.selectedImage = imageSrc;
      this.isModalOpen = true;
    },

    // Fermer le modal
    closeImageModal() {
      this.isModalOpen = false;
      this.selectedImage = null;
    }
  }
};
</script>


<style>
.image-grid-2 {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin: 0 20px;
}

.card-2 {
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  max-width: 400px;
}

.img-card-2 {
  height: 100px;
  max-width: 280px;
  border-radius: 5px;
  cursor: pointer;
}

.text-card-2 {
  padding-top: 10px;
  font-size: 16px;
  cursor: pointer;
}

.text-card-2:hover {
  color: gray;
}

.category-card-2 {
  font-size: 12px;
  color: rgb(196, 196, 196);
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  flex-direction: column;
}
.modal-image {
  max-width: 80%;
  max-height: 80%;
}


</style>
