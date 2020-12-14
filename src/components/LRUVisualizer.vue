<template>
  <div class="visualizer-main-wrapper">
    <div class="queue-wrapper">
      <div class="page-list-wrapper">
        Текущая очередь:
        <div
            v-for="(page, index) in pagesListCopy"
            :key="index"
            class="page-container"
        >
          {{page}}
        </div>
      </div>
    </div>
    <div class="visualisation-container">
      <div class="memory-container"
           v-if="queueSize !== undefined && queueSize.length !== 0"
      >
        <div
            v-for="i in queueSize"
            :key="i"
        >
          <div class="memory-page-container">
            {{buffer[i-1]}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

export default {
  name: "LRUVisualizer",
  props: ['pagesList', 'queueSize', 'delay'],
  data: () => ({
    buffer: [],
    pagesListCopy: [],

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
        if (this.buffer[0] !== el) {
          let findEl = false;
          for (let i = this.buffer.length - 1; i >= 0; --i) {
            if (!findEl && this.buffer[i] !== el){
              continue;
            } else {
              findEl = true;
              if (i !== 0) {
                this.buffer[i] = this.buffer[i - 1]
              } else {
                this.buffer[0] = el;
              }
            }
          }
          this.buffer[0] = el;
        } else {
          this.buffer[0] = el;
        }
      } else {
        let emptyEls = this.buffer.filter(x => x===' ').length;
        if (emptyEls !== 0){
          this.buffer[this.queueSize - (emptyEls)] = el;
        } else {
          for (let i = this.buffer.length - 1; i !== 0; --i) {
            this.buffer[i] = this.buffer[i-1];
          }
          this.buffer[0] = el;
        }
      }
    },
    add: function (){
      if (this.pagesListCopy.length !== 0) {
        let el = this.pagesListCopy[this.pagesListCopy.length - 1]
        this.pagesListCopy.splice(-1, 1);
        this.addToQueue(el);
      }
    },
    reInit: function (){
      console.log('reinit');
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
.visualisation-container{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.memory-container{
  border: 2px solid rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: row;
}
.memory-page-container{
  width: 60px;
  height: 60px;
  border-left: 2px solid rgba(0, 0, 0, 0.2);
  border-right: 2px solid rgba(0, 0, 0, 0.2);
  font-size: 45px;
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
  height: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: baseline;
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