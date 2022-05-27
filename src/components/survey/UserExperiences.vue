<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperience">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No stored experiences found. Start adding some survey results</p>
      <ul v-else-if="!isLoading && results && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';
import axios from 'axios';

export default {
  mounted() {
    this.loadExperience()
  },
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null
    }
  },
  methods: {
    loadExperience() {
      this.isLoading = true
      this.error = null

      axios.get('https://http-vue-20891-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json')
      .then(response => {
        this.isLoading = false

        const data = response.data
        const results = []

        for (const id in data) {
          results.push({
            id: id, 
            name: data[id].name,
            rating: data[id].rating
          })
        }

        this.results = results
      })
      .catch(error => {
        console.log(error);
        this.isLoading = false
        this.error = 'Failed to fetch data. Please try again later '
      })
    }
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>