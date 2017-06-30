<template>
  <div class="timeline">
    <h1 v-if ="loading">Chargement en cours...</h1>
    <ul v-else>
      <h1> {{ this.utilisateur ? 'Bonjour ' + this.utilisateur + ' ! ' : 'Identifiez-vous !!' }} </h1>
      <utilisateurs :utilisateurs="utilisateurs" @userChanged="onChange"/>
      <feed :tweets="tweets" :loading="loading" :utilisateur="utilisateur" @retweeted="retweet"></feed>
    </ul>
  </div>
</template>

<script>
import Feed from './Feed'
import Utilisateurs from './Utilisateurs'
import Vue from 'vue'
import Resource from 'vue-resource'
Vue.use(Resource)

export default {
  name: 'timeline',
  components: {Feed, Utilisateurs},
  props: ['utilisateur'],
  created () {
    this.fetchTweets()
    this.fetchUtilisateurs()
  },
  methods: {

    retweet: function (id) {
      var tweet = this.tweets.find(tweet => id === tweet.id)
      tweet.retweeters.push({handle: this.utilisateur})
      for (var i = 0; i < this.tweets.length; i++) {
        if (this.tweets[i].id === id) {
          this.tweets[i].retweeters.push({handle: this.utilisateur})
        }
      }
    },

    fetchTweets: function () {
           // GET /someUrl
      this.$http.get('http://localhost:8080/list').then(response => {
        // get body data
        this.tweets = response.body
        this.loading = false
      }, response => {
             // error callback
      })
    },

    onChange: function (handle) {
      this.utilisateur = handle
    },

    fetchUtilisateurs: function () {
               // GET /someUrl
      this.$http.get('http://localhost:8080/utilisateurs').then(response => {
            // get body data
        this.utilisateurs = response.body
      }, response => {
                 // error callback
      })
    }

  },

  data () {
    return {

      tweets: [],
      loading: true,
      utilisateurs: []

    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
a {
  color: #42b983;
}

</style>
