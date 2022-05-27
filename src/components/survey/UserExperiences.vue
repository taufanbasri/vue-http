<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperience">Load Submitted Experiences</base-button>
      </div>
      <ul>
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
      results: []
    }
  },
  methods: {
    loadExperience() {
      axios.get('https://http-vue-20891-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json')
      .then(response => {
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
      .catch(function (error) {
        console.log(error);
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