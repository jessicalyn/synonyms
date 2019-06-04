<template>
  <div id="app">
    <Header msg="SYNONYMS"/>
    <form>
      <input type="text" v-model="searchWord" />
      <button v-on:click.prevent="buildUrl">find synonyms</button>
    </form>
    <h3>{{searchWord}}</h3>
  </div>
</template>

<script>
import Header from './components/Header.vue'

export default {
  name: 'app',
  components: {
    Header
  },
  data: function() {
    return { 
      searchWord: null 
    }
  },
  methods: {
    buildUrl: function() {
      const url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${this.searchWord}?key=c0ab8f16-fea9-482d-bc45-75a907b628b3`
      this.searchFetch(url)
    },
    async searchFetch(url) {
      try {
        const response = await fetch(url)
        if (!response.ok) {
          throw Error (response.statusText)
        }
        const result = await response.json()
        const synonyms = result[0].meta.syns.flat()
        this.displayResults(synonyms)
      } catch (error) {
        console.log(error)
      }
    },
    displayResults(synonyms) {
      console.log(synonyms)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 0;
  box-sizing: border-box;
}
</style>
