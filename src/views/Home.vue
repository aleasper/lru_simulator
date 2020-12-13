<template>
  <div class="home home-div">
    <div class="lru-viewer-div">
      <LRUVisualizer
          ref="LRUVisualizer"
          v-bind:pages-list="arrayPages"
          v-bind:queue-size="queueSize"
          v-bind:delay="delay"
      ></LRUVisualizer>
    </div>
    <div class="vertical-divider">
    </div>
    <div class="lru-settings-div">
      <h1>Исходные данные</h1>
      <div class="queue-size-input">
        <v-text-field
            label="Размер очереди"
            v-model="queueSize"
            outlined
        ></v-text-field>
      </div>
      <div>
        <v-text-field
            label="Страницы через запятую"
            v-model="pages"
            :rules="[pagesRule]"
            outlined
        ></v-text-field>
        <div
            v-if="arrayPages.length > 0"
            class="pages-viewer-div"
        >
          <div>Введённые страницы: </div>
          <div
              v-for="(page, index) in arrayPages"
              :key="index"
              class="page-view"
          >{{page}}</div>
        </div>
      </div>
      <div>
        <v-slider
            label="Задержка между командами в мс"
            max="1000"
            min="10"
            thumb-label
            v-model="delay"
        ></v-slider>
      </div>
      <div>
        <v-btn v-on:click="start">Старт</v-btn>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import LRUVisualizer from "@/components/LRUVisualizer";

export default {
  name: 'Home',
  components: {
    LRUVisualizer
  },
  data:  () => ({
    queueSize: '',
    pages: '',
    arrayPages: [],
    delay: 10,
  }),
  methods: {
    pagesRule: function (){
      this.arrayPages = [];
      let t  = this.pages.split(',');
      for (let i in t){
        if (t[i] !== '' && t[i] !== ' '){
          this.arrayPages.push(t[i]);
        }
      }
      return true;
    },
    start: function (){
      this.$refs.LRUVisualizer.start();
    }
  },
  watch: {
  }
}
</script>
<style scoped>
.home-div{
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
}
.lru-viewer-div{
  height: 100%;
  width: 70%;
}
.lru-settings-div{
  height: 100%;
  width: calc(30% - 2px);

  display: flex;
  flex-direction: column;
  padding: 8px;
}
.vertical-divider{
  height: 100%;
  width: 2px;
  background: #BBDEFB;
}
.queue-size-input{
  display: flex;
  flex-direction: row;
}
.pages-viewer-div{
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.page-view{
  border: 1px solid rgba(0, 0, 0, 0.5);
  border-radius: 4px;
  padding: 2px;
  margin: 2px;
}
</style>
