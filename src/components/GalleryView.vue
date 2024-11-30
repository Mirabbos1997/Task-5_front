<template>
  <div class="gallery">
    <div v-for="book in books" :key="book.index" class="gallery-item">
      <div class="book-cover">Cover Image Placeholder</div>
      <div class="book-details">
        <h3>{{ book.title }}</h3>
        <p><strong>Author:</strong> {{ book.authors }}</p>
        <p><strong>Publisher:</strong> {{ book.publisher }}</p>
      </div>
    </div>
    <div v-if="isLoading">Loading...</div>
  </div>
</template>

<script>
export default {
  props: ["books", "isLoading"],
};
</script>

<style>
.gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
.gallery-item {
  border: 1px solid #ddd;
  padding: 16px;
  width: calc(33.333% - 32px);
  box-sizing: border-box;
}
.book-cover {
  height: 150px;
  background-color: #f0f0f0;
  margin-bottom: 8px;
}
</style>