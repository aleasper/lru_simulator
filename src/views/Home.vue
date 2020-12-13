<template>
  <div class="home home-div">
    <div class="lru-viewer-div">

    </div>
    <div class="vertical-divider">

    </div>
    <div class="lru-settings-div">

    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home',
  components: {
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
}
.vertical-divider{
  height: 100%;
  width: 2px;
  background: #BBDEFB;
}
</style>
