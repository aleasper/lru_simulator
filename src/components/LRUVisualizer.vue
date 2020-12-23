<template>
  <div class="visualizer-main-wrapper">
    <div class="queue-wrapper">
      <div class="page-list-wrapper">
        Текущая очередь:
        <div
            v-for="(page, index) in pagesListCopy"
            :key="index"
            class="page-container"
            :style="{'background': index === pagesListCopy.length - 1 ? 'rgba(255, 0, 0, 0.5)': 'none'}"
        >
          {{page}}
        </div>
        <div v-if="pagesListCopy.length !== 0" class="page-list-arrow"></div>
      </div>
    </div>
    <div class="visualisation-container">
      <div class="memory-container"
           v-if="queueSize !== undefined && queueSize.length !== 0"
      >
        <div class="page-list-arrow"></div>
        <div
            v-for="i in queueSize"
            :key="i"
        >
          <div class="memory-page-container"
               :style="{'background': pagesListCopy.length !== 0 && buffer[i-1] === pagesListCopy[pagesListCopy.length - 1] ? 'rgba(0, 255, 0, 0.5)': 'none'}"
          >
            {{buffer[i-1]}}
          </div>
        </div>
        <div class="page-list-arrow"></div>
      </div>
    </div>
    <div class="fault-counter-div">
      Количество прерываний (вставок в буфер новых страниц): {{breaks}}
    </div>
    <div v-if="description" class="description-container">
      <div style="font-size: 18px; margin: 4px 0">
        Текстовое описание шагов
      </div>
      <div
          v-for="(str, i) in descriptionText"
          :key="i"
      >{{str}}</div>
    </div>
  </div>
</template>

<script>
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

export default {
  name: "LRUVisualizer",
  props: ['pagesList', 'queueSize', 'delay', 'description'],
  data: () => ({
    buffer: [],
    pagesListCopy: [],
    descriptionText: [],
    breaks: 0,
  }),
  methods: {
    start: async function (){
      this.reInit();
      await sleep(this.delay);
      for (let i = 0; i !== this.pagesList.length; ++i){
        await sleep(this.delay);
        this.add();
      }
    },
    addToQueue: function (el){
      if (this.buffer.includes(el)){
        this.descriptionText.push(`Страница "${el}" уже есть в буфере`)
        if (this.buffer[0] !== el) {
          let findEl = false;
          for (let i = this.buffer.length - 1; i >= 0; --i) {
            if (!findEl && this.buffer[i] !== el){
              continue;
            } else {
              if (!findEl) {
                this.descriptionText.push(`Нашли страницу на ${i + 1} позиции, сдвигаем страницы, которые находятся левее на одну ячейку вправо`)
              }
              findEl = true;
              if (i !== 0) {
                this.buffer[i] = this.buffer[i - 1]
              } else {
                this.descriptionText.push(`Перемещаем "${el}" в первую ячейку буфера`)
                this.buffer[0] = el;
              }
            }
          }
          this.buffer[0] = el;
        } else {
          this.descriptionText.push(`Страница "${el}" уже находится в первой ячейке`)
          this.buffer[0] = el;
        }
      } else {
        this.breaks = this.breaks + 1;
        this.descriptionText.push(`Сдвигаем вправо все страницы и добавляем "${el}" в начало буфера`)
        for (let i = this.buffer.length - 1; i !== 0; --i) {
          this.buffer[i] = this.buffer[i-1];
        }
        this.buffer[0] = el;
      }
    },
    add: function (){
      if (this.pagesListCopy.length !== 0) {
        let el = this.pagesListCopy[this.pagesListCopy.length - 1]
        this.descriptionText.push(`Загружаем в буфер страницу ${el}`)
        this.pagesListCopy.splice(-1, 1);
        this.addToQueue(el);
      }
    },
    reInit: function (){
      console.log('reinit');
      this.breaks = 0;
      this.descriptionText = [];
      this.buffer = [];
      for (let i = 0; i !== this.queueSize; ++i){
        this.buffer.push(' ');
      }
      this.pagesListCopy = [...this.pagesList];
    }
  },
  mounted() {
    this.reInit();
  },
}
</script>

<style scoped>
.fault-counter-div{
  padding: 10px;
}
.description-container{
  padding: 4px 12px;
  font-size: 12px;
  max-height: 300px;
  overflow-y: auto;
  margin-top: 30px;
}
.page-list-arrow{
  background-image: url(../assets/arrow.png);
  background-repeat: no-repeat;
  background-size: cover;
  margin: 4px;
  height: 40px;
  width: 40px;
}
.visualisation-container{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.memory-container{
  /*border: 2px solid rgba(0, 0, 0, 0.2);*/
  display: flex;
  flex-direction: row;
  align-items: center
}
.memory-page-container{
  width: 70px;
  height: 70px;
  border: 2px solid rgba(0, 0, 0, 0.2);
  /*border-right: 2px solid rgba(0, 0, 0, 0.2);*/
  font-size: 55px;
  text-align: center;
}
.visualizer-main-wrapper{
  width: 100%;
  height: 100%;
}
.queue-wrapper{
  height: 30%;
  width: 100%;
  padding: 10px;
}
.page-list-wrapper{
  width: 100%;
  height: 60%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
}
.page-container{
  font-size: 26px;
  margin: 4px;
  padding: 2px;
  border: 1px solid rgba(0,0,0, 0.1);
  border-radius: 4px;
  height: fit-content;
  width: fit-content;
}
</style>