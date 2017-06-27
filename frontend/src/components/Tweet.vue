<template>
  <div class="tweet">
    <h1>{{ msg }}</h1>
    <div>{{ tweet.auteur.prenom }} {{ tweet.auteur.nom }} : {{ tweet.auteur.handle }}</div>
  </div>
</template>

<script>
export default {
  name: 'tweet',
  props: ['tweet']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
