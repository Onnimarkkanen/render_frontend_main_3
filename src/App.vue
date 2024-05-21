<!-- App.vue -->
<template>
  <div id="app">
    <h1>Sentiment Analysis</h1>
    <textarea v-model="inputText" placeholder="Enter text to analyze"></textarea>
    <button @click="analyze">Analyze</button>
    <div v-if="loading" class="loading">Loading...</div>
    <div v-if="sentiment" class="result">
      <h2>Analysis Result</h2>
      <p>Sentiment: {{ sentiment.label }}</p>
      <p>Confidence: {{ sentiment.confidence }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      inputText: '',
      sentiment: null,
      loading: false
    };
  },
  methods: {
    analyze() {
      if (this.inputText.trim() === '') {
        alert('Please enter some text.');
        return;
      }
      this.loading = true;
      axios.post('https://render-frontend-main-3.onrender.com', {
        text: this.inputText
      })
      .then(response => {
        this.sentiment = response.data;
      })
      .catch(error => {
        console.error('Error analyzing sentiment:', error);
      })
      .finally(() => {
        this.loading = false;
      });
    }
  }
};
</script>

<style>
#app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}
textarea {
  width: 100%;
  height: 150px;
  margin-bottom: 10px;
}
button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
}
.loading {
  margin-top: 20px;
}
.result {
  margin-top: 20px;
}
</style>
