<template>
  <div class="visualizer-main-wrapper">
    <div class="queue-wrapper">
      <div class="page-list-wrapper">
        Текущая очередь:
        <div
            v-for="(page, index) in pagesList"
            :key="index"
            class="page-container"
        >
          {{page}}
        </div>
      </div>

    </div>
    <div>

    </div>
  </div>
</template>

<script>
export default {
  name: "LRUVisualizer",
  props: ['pagesList', 'queueSize', 'delay'],
  data: () => ({

  }),
  methods: {
    start: function (){
      console.log('start!');
    }
  }
}
</script>

<style scoped>
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