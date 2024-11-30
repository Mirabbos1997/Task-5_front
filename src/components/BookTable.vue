<template>
  <div>
    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>ISBN</th>
          <th>Title</th>
          <th>Authors</th>
          <th>Publisher</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <template v-for="book in books" :key="book.index">
          <!-- Main Row -->
          <tr @click="toggleExpand(book.index)">
            <td>{{ book.index }}</td>
            <td>{{ book.isbn }}</td>
            <td>{{ book.title }}</td>
            <td>{{ book.authors }}</td>
            <td>{{ book.publisher }}</td>
            <td class="toggle-icon">
              <span v-if="expandedRow === book.index">▼</span>
              <span v-else>▲</span>
            </td>
          </tr>
          <!-- Accordion Row -->
          <tr v-if="expandedRow === book.index" class="expanded-row">
            <td colspan="6">
              <div class="details-container">
                <h4>{{ book.title }}</h4>
                <p><strong>Author:</strong> {{ book.authors }}</p>
                <p><strong>Publisher:</strong> {{ book.publisher }}</p>
                <p>
                  <strong>Review:</strong>
                  {{ book.review || "No reviews available" }}
                </p>
              </div>
            </td>
          </tr>
        </template>
      </tbody>
    </table>
    <div v-if="isLoading" class="loading">Loading...</div>
    <div class="button-container">
      <button class="button load-more" @click="$emit('load-more')">
        Load More
      </button>
      <button class="button export-csv" @click="$emit('export-csv')">
        Export to CSV
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["books", "isLoading"],
  data() {
    return {
      expandedRow: null, // Track the currently expanded row
    };
  },
  methods: {
    toggleExpand(index) {
      // Toggle the expanded row
      this.expandedRow = this.expandedRow === index ? null : index;
    },
  },
};
</script>

<style>
/* General Table Styling */
table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  overflow: hidden;
}

thead th {
  background-color: #007bff;
  color: white;
  text-align: left;
  padding: 12px;
  font-size: 1rem;
}

tbody tr {
  background-color: white;
  transition: background-color 0.3s ease;
  cursor: pointer;
}

tbody tr:hover {
  background-color: #f9f9f9;
}

tbody td {
  padding: 12px;
  border-top: 1px solid #ddd;
}

.toggle-icon {
  text-align: center;
  font-size: 1.2rem;
  color: #007bff;
  font-weight: bold;
  cursor: pointer;
}

.expanded-row td {
  background-color: #f9f9f9;
  padding: 20px;
  border: none;
}

.details-container {
  padding: 15px;
  border-left: 5px solid #007bff;
  background-color: #f9f9f9;
  border-radius: 5px;
}

.details-container h4 {
  margin: 0 0 10px;
  font-size: 1.2rem;
  color: #007bff;
}

.details-container p {
  margin: 5px 0;
  font-size: 0.9rem;
  color: #555;
}

/* Buttons Styling */
.button-container {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}

.button {
  padding: 10px 20px;
  font-size: 1rem;
  color: white;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.button:hover {
  background-color: #0056b3;
  transform: translateY(-2px);
}

.button:active {
  transform: translateY(0);
}

.load-more {
  background-color: #28a745;
}

.load-more:hover {
  background-color: #218838;
}

.export-csv {
  background-color: #17a2b8;
}

.export-csv:hover {
  background-color: #138496;
}

/* Loading Indicator Styling */
.loading {
  font-size: 1.2rem;
  color: #6c757d;
  margin: 10px 0;
  text-align: center;
}
</style>
