<template>
  <div class="home home-div">
    <div class="lru-viewer-div">
      <LRUVisualizer
          ref="LRUVisualizer"
          v-bind:pages-list="arrayPages"
          v-bind:queue-size="parseInt(queueSize)"
          v-bind:delay="parseInt(delay)"
          v-bind:description="description"
      ></LRUVisualizer>
    </div>
    <div class="vertical-divider">
    </div>
    <div class="lru-settings-div">
      <h1>Исходные данные</h1>
      <div class="queue-size-input">
        <v-select
            :items="queueSizeList"
            label="Размер очереди"
            outlined
            v-model="queueSize"
        ></v-select>
      </div>
      <div>
        <v-text-field
            label="Страницы через запятую"
            v-model="pages"
            :rules="[pagesRule]"
            outlined
        ></v-text-field>
        <div class="upload-div">
          <v-btn
              color="blue-grey"
              class="white--text"
              v-on:click="openFile"
          >
            Загрузить из файла
            <v-icon
                right
                dark
            >
              mdi-cloud-upload
            </v-icon>
          </v-btn>
          <input id="file-input" type="file" style="display: none" accept=".txt"/>
        </div>
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
            max="2000"
            min="100"
            thumb-label
            v-model="delay"
        ></v-slider>
      </div>
      <div>
        <v-checkbox
            v-model="description"
            label="Текстовое описание шагов"
        ></v-checkbox>
      </div>

      <div class="actions-div">
        <v-btn class="action-btn" v-on:click="autoStart">Автоматический старт</v-btn>
        <div class="manual-actions-div">
          <v-btn class="action-btn" v-on:click="loadData">Загрузить данные</v-btn>
          <v-btn class="action-btn" v-on:click="step">Сделать один шаг</v-btn>
          <v-btn class="action-btn" v-on:click="reStart">Сбросить</v-btn>
        </div>

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
    queueSize: 5,
    pages: 'A, F, C, B, D, E, A, C, A, C',
    arrayPages: [],
    delay: 100,
    queueSizeList: [2, 3, 4, 5, 6, 7, 8],
    description: false,
  }),
  methods: {
    pagesRule: function (){
      this.arrayPages = [];
      let temp = this.pages.replaceAll(' ', '');
      let t  = temp.split(',');

      for (let i in t){
        if (t[i] !== '' && t[i] !== ' '){
          this.arrayPages.push(t[i]);
        }
      }
      return true;
    },
    autoStart: function (){
      if (this.pages.replaceAll(' ', '') !== ''){
        this.$refs.LRUVisualizer.start();
      }
    },
    step: function (){
      if (this.pages.replaceAll(' ', '') !== ''){
        this.$refs.LRUVisualizer.add();
      }
    },
    loadData: function (){
      if (this.pages.replaceAll(' ', '') !== ''){
        this.$refs.LRUVisualizer.reInit();
      }
    },
    reStart: function (){
      if (this.pages.replaceAll(' ', '') !== ''){
        this.$refs.LRUVisualizer.reInit();
      }
    },
    openFile(){
      let self = this;
      document.getElementById('file-input').click();
      document.getElementById('file-input').addEventListener('change', (event) => {
        const file = event.target.files[0];
        const reader = new FileReader();
        reader.addEventListener('load', (event) => {
          let base64 = event.target.result.split(',')[1];
          let buff = new Buffer(base64, 'base64');
          let text = buff.toString();
          self.pages = text;
        });
        reader.readAsDataURL(file);
      });
    }
  },
  watch: {
  }
}
</script>
<style scoped>
.upload-div{
  margin: 4px 0 8px 0;
}
.action-btn{
  margin: 2px;
}
.manual-actions-div{
  margin: 10px 0;
  display: flex;
  flex-direction: column;
}
.actions-div{
  display: flex;
  flex-direction: column;
}
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
