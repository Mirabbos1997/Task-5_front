<template>
  <div class="controls">
    <select v-model="selectedLang">
      <option value="en_US">English (US)</option>
      <option value="fr">Français (FR)</option>
      <option value="de">Deutsch (DE)</option>
    </select>
    <input v-model="seed" placeholder="Enter seed" />
    <button @click="$emit('random-seed')">Generate Random Seed</button>
    <div>
      <label>Average Likes:</label>
      <input type="range" v-model="avgLikes" min="0" max="10" step="0.1" />
    </div>
    <div>
      <label>Average Reviews:</label>
      <input type="number" v-model="avgReviews" min="0" max="10" step="0.1" />
    </div>
    <button @click="updateSettings">Update</button>
  </div>
</template>

<script>
export default {
  props: ["languages"],
  data() {
    return {
      selectedLang: "en_US",
      seed: "",
      avgLikes: 5,
      avgReviews: 5,
    };
  },
  methods: {
    updateSettings() {
      this.$emit("update", {
        seed: this.seed,
        lang: this.selectedLang,
        avgLikes: this.avgLikes,
        avgReviews: this.avgReviews,
      });
    },
  },
};
</script>