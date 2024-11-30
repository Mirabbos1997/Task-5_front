<template>
  <div id="app">
    <Controls
      :languages="languages"
      @update="handleUpdate"
      @random-seed="generateRandomSeed"
    />
    <div class="view-toggle">
      <button @click="toggleView('table')">Table View</button>
      <button @click="toggleView('gallery')">Gallery View</button>
    </div>
    <BookTable
      v-if="viewMode === 'table'"
      :books="books"
      :isLoading="isLoading"
      @load-more="loadMoreBooks"
      @export-csv="exportBooksToCSV"
    />
    <GalleryView
      v-else
      :books="books"
      :isLoading="isLoading"
    />
  </div>
</template>

<script>
import axios from 'axios';
import Controls from './components/Controls.vue';
import BookTable from './components/BookTable.vue';
import GalleryView from './components/GalleryView.vue';
import Papa from 'papaparse';

export default {
  components: {
    Controls,
    BookTable,
    GalleryView,
  },
  data() {
    return {
      languages: ['en_US', 'de', 'fr'],
      seed: 'default-seed',
      page: 1,
      lang: 'en_US',
      avgLikes: 5,
      avgReviews: 5,
      books: [],
      isLoading: false,
      viewMode: 'table',
    };
  },
  methods: {
    async fetchBooks() {
      this.isLoading = true;
      try {
        const response = await axios.post('http://localhost:3000/api/books', {
          seed: this.seed,
          page: this.page,
          lang: this.lang,
          avgLikes: this.avgLikes,
          avgReviews: this.avgReviews,
        });
        this.books = this.page === 1 ? response.data : [...this.books, ...response.data];
      } catch (error) {
        console.error('Error fetching books:', error);
      } finally {
        this.isLoading = false;
      }
    },
    handleUpdate({ seed, lang, avgLikes, avgReviews }) {
      this.seed = seed;
      this.lang = lang;
      this.avgLikes = avgLikes;
      this.avgReviews = avgReviews;
      this.page = 1;
      this.fetchBooks();
    },
    generateRandomSeed() {
      this.seed = Math.random().toString(36).substring(7);
      this.page = 1;
      this.fetchBooks();
    },
    loadMoreBooks() {
      this.page++;
      this.fetchBooks();
    },
    exportBooksToCSV() {
      const csv = Papa.unparse(this.books);
      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.download = 'books.csv';
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    toggleView(mode) {
      this.viewMode = mode;
    },
  },
  mounted() {
    this.fetchBooks();
  },
};
</script>