<template>
  <section class="score"> Jogador <span v-html="playerPoints"></span> x <span v-html="computerPoints"></span> Computador
  </section>
</template>

<script>
export default {
  props: ['playerPoints', 'computerPoints']
}
</script>

<style lang="scss" scoped>
section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;
}
</style>
