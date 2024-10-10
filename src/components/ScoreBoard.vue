<template>
  <section class="score">
    Player <span>{{ this.winCount }}</span> x 
   <span>{{ this.loseCount }}</span> Computer
  </section>
</template>

<script>

export default {
    props: ['winCount', 'loseCount']
}
</script>

<style scoped lang="scss">
section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;

  span {
    padding: 10px;
    font-weight: bold;
    border: 2px solid black;
  }
}
</style>