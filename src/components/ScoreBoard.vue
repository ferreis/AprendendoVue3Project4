<template>
  <section class="score"> Jogador <span>0</span> x <span>0</span> Computador </section>
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
