<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul>
    <li v-for="tweet in tweets">
      <tweet :tweet="tweet"/>
    </li>
    </ul>
  </div>
</template>

<script>
import Tweet from './Tweet'
import Vue from 'vue'
import Resource from 'vue-resource'
Vue.use(Resource)
export default {
  name: 'timeline',
  components: {Tweet},
  created () {
    this.fetchTweets()
  },
  methods: {

    fetchTweets: function () {
           // GET /someUrl
      this.$http.get('http://localhost:8080/list').then(response => {
        // get body data
        this.tweets = response.body
      }, response => {
             // error callback
      })
    }
  },

  data () {
    return {

      tweets: [
        { auteur: 'Yamine', contenu: 'Salut' },
        { auteur: 'Gaetan', contenu: 'Aiiiiight' },
        { auteur: 'Olivier', contenu: 'Wesh' }
      ]

    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  margin: 0 10px;
}
</style>
