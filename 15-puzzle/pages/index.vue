<template>
  <div class="main">
    <h2>Добро пожаловать в мое первое приложение на Vue</h2>
    <Game />
  </div>
</template>

<script>
import GamePart from "@/components/game";

export default {
  components: {
    GamePart,
  },
};
</script>

<style lang="scss">
.main {
  background-color: #f5edfa;
  height: 1000px;
  padding: 2rem;
  color: #696573;

  h2 {
    margin-bottom: 20px;
  }
}
</style>


