<template>
  <div id="app">
    <Header msg="SYNONYMS"/>
    <form>
      <input type="text" v-model="searchWord" />
      <button v-on:click.prevent="buildUrl">find synonyms</button>
    </form>
    <h3>{{searchWord}}</h3>
    <ul>
      <li
        v-for="(synonym, index) in searchResults"
        v-bind:key="index"
        v-on:click="newSyn"
      >
        {{synonym}}
      </li>
    </ul>
    {{error}}
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
      searchWord: null,
      searchResults: [],
      error: ""
    }
  },
  methods: {
    buildUrl: function() {
      this.error = ""
      const apiKey = process.env.VUE_APP_API_KEY
      const url = `https://www.dictionaryapi.com/api/v3/references/thesaurus/json/${this.searchWord}?key=${apiKey}`
      this.searchFetch(url)
    },
    async searchFetch(url) {
      try {
        const response = await fetch(url)
        if (!response.ok) {
          throw Error (response.statusText)
        }
        const result = await response.json()
        if(result.length === 0) {
          return this.error = "Sorry we can't find synonyms for that word. Try another!"
        }
        const synonyms = result[0].meta.syns.flat()
        this.searchResults = synonyms
      } catch (error) {
        this.error = error.message
      }
    },
    newSyn: function(e) {
      const newWord = e.target.innerText
      this.searchWord = newWord
      this.buildUrl()
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
