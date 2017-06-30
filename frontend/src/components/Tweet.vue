<template>
  <div class="tweet">
    <div>
    <strong>{{ tweet.auteur.prenom }} {{ tweet.auteur.nom }}</strong> :
    <span class="handle">{{ tweet.auteur.handle }} - {{moment(tweet.date).fromNow()}} </span>
    </div>
    <div>
        {{ tweet.contenu}}
    </div>
    <div>
      <ul>
        <li class="button"> <icon name="reply"/> </li>
        <a @click="retweet()"><li class="button"><icon name="retweet"/> </li></a>
        {{ tweet.retweeters.length }}
        <li class="button"> <icon name="heart"/> </li>
        <li class="button"> <icon name="envelope"/> </li>
      </ul>
    </div>
  </div>
</template>


<script>
import Vue from 'vue'
import Resource from 'vue-resource'
Vue.use(Resource)
import 'vue-awesome/icons'
import Icon from 'vue-awesome/components/Icon'
import moment from 'moment'

export default {
  name: 'tweet',
  props: ['tweet'],
  components: {Icon},

  methods: {

    moment: function (date) {
      return moment(date)
    },
    retweet: function () {
                   // GET /someUrl
      this.$http.get('http://localhost:8080/retweet', {params: {utilisateur: 'snoopdog', tweet: this.tweet.id}, responseType: 'text'}).then(response => {
                // get body data
        this.tweets = response.body
        this.loading = false
        this.$emit('retweeted', this.tweet.id)
      }, response => {
             // error callback
      })
    }
  },
  created () {
    moment.locale('fr')
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
li.button {
 display: inline-block;
}

a {
 color: #42b983;
}

span.handle {
 color: gray;
}

</style>

