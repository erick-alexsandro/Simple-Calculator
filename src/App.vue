<script setup>
import Calculator from "./components/Calculator.vue";
</script>

<template>
  <div id="title">
    <h1>Calculator</h1>
    <p>Created by Erick Alexsandro</p>
  </div>
  <div id="app">
    <Calculator />
  </div>
</template>

<style scoped>
#title h1,
p {
  color: #ebe9fc;
  font-family: "Inter var experimental", "Inter var", Inter, -apple-system,
    BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell,
    "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  font-weight: 800;
}

#title p {
  filter: brightness(90%);
}

#title h1 {
  font-size: 70px;
}
</style>
