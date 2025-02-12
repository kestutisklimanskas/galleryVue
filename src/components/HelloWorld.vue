<template>
  <div class="container">
    
    <div class="row mb-3">
      <div class="col">
        <input v-model="filters.title" placeholder="Filter by Title" class="form-control" />
      </div>
      <div class="col">
        <input v-model="filters.id" placeholder="Filter by ID" type="number" class="form-control" />
      </div>
      <div class="col">
        <select v-model="filters.category" class="form-control">
          <option value="">All Categories</option>
          <option v-for="cat in uniqueCategories" :key="cat">{{ cat }}</option>
        </select>
      </div>
    </div>

    <div v-if="!selectedImage" class="row">
      <div v-for="image in filteredImages" :key="image.id" class="col-md-3 mb-3">
        <div class="card" @click="selectImage(image)">
          <img :src="image.src" :alt="image.alt" class="card-img-top" />
          <div class="card-body text-center">
            <h6 class="card-title">{{ image.title }}</h6>
          </div>
        </div>
      </div>
    </div>

   
    <div v-else class="detail-view">
      <button class="btn btn-secondary mb-2" @click="selectedImage = null">Back</button>
      <div class="card">
        <img :src="selectedImage.src" class="card-img-top" />
        <div class="card-body">
          <h5>{{ selectedImage.title }}</h5>
          <p>{{ selectedImage.description }}</p>
          <p><strong>Category:</strong> {{ selectedImage.category.join(", ") }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      images: [
        { id: 1, src: "https://via.placeholder.com/150", alt: "Image 1", category: ["Nature"], title: "Sunset", description: "Beautiful sunset." },
        { id: 2, src: "https://via.placeholder.com/150", alt: "Image 2", category: ["City"], title: "Skyscrapers", description: "Tall buildings in the city." },
        { id: 3, src: "https://via.placeholder.com/150", alt: "Image 3", category: ["Nature"], title: "Mountain", description: "A scenic mountain view." },
        { id: 4, src: "https://via.placeholder.com/150", alt: "Image 4", category: ["Animals"], title: "Elephant", description: "An elephant in the wild." }
      ],
      filters: { title: "", id: "", category: "" },
      selectedImage: null
    };
  },
  computed: {
    filteredImages() {
      return this.images.filter((img) => {
        return (
          (!this.filters.title || img.title.toLowerCase().includes(this.filters.title.toLowerCase())) &&
          (!this.filters.id || img.id == this.filters.id) &&
          (!this.filters.category || img.category.includes(this.filters.category))
        );
      });
    },
    uniqueCategories() {
      return [...new Set(this.images.flatMap((img) => img.category))];
    }
  },
  methods: {
    selectImage(image) {
      this.selectedImage = image;
    }
  }
};
</script>

<style>
.card {
  cursor: pointer;
  transition: transform 0.2s;
}
.card:hover {
  transform: scale(1.05);
}
.detail-view {
  text-align: center;
}
</style>
