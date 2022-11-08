<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperience"
          >Load Submitted Experiences</base-button
        >
      </div >
      <h1 v-if="isLoading">loading...</h1>
      <p v-else-if="!isLoading && error"> {{error}} </p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No data</p>
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

export default {
  data() {
    return {
      results: [],
      isLoading: false,
      error : null
    };
  },
  components: {
    SurveyResult,
  },
  methods: {
    loadExperience() {
      this.isLoading = true
      fetch(
        'https://vue-https-b5687-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json'
      )
        .then((respone) => {
          if (respone.ok) {
            return respone.json();
          }
        })
        .then((data) => {
          this.isLoading = false
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            })
          }
            this.results = results
          })
          .catch((error) => {
          console.log("errrr" + "  "  +error)
          this.isLoading = false
          this.error = error
        });
    },
  },
  mounted() {
    this.loadExperience();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
