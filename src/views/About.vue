<template>
  <div class="about">
    <h3>Это web-приложение создано с целью демонстрации работы алгоритма вытеснения страниц "LRU"</h3>
    <div style="height: 40px"></div>
    <div>
      Исходный код: <a target="_blank" href="https://github.com/aleasper/lru_simulator">https://github.com/aleasper/lru_simulator</a><br>
      Автор: <a target="_blank" href="https://vk.com/alexandrvasilchenko">Васильченко Александр</a><br><br>
      Стэк технологий: Vue2 + Vuetify
    </div>
  </div>
</template>

<script>
export default {
  name: "About"
}
</script>

<style scoped>
.about{
  padding: 8px 12px;
  font-size: 18px;
}
</style>
