<template>
  <div id="app">
    <CheckerBoard
    ref="checkerboard"
    :xNum="xNum"
    :yNum="yNum"
    :posibility="posibility"
    :flag="flag"
    class="checkerboard"/>
    <div class="controls">
      <div>
        水平方向格数
        <InputNumber
          :max="16"
          :min="3"
          size="large"
          v-model="xNum">
        </InputNumber>
      </div>
      <div>
        竖直方向格数
        <InputNumber
          :max="12"
          :min="3"
          size="large"
          v-model="yNum">
        </InputNumber>
      </div>
      <div>
        雷出现的概率
        <InputNumber
          :max="1"
          :min="0"
          :step="0.01"
          size="large"
          v-model="posibility">
        </InputNumber>
      </div>
      <div>
        插旗模式
        <i-switch v-model="flag" />
      </div>
      <Button
        long
        size="large"
        @click="init">
        重置
      </Button>
    </div>
  </div>
</template>

<script>
import CheckerBoard from './components/CheckerBoard.vue'

export default {
  name: 'app',
  components: {
    CheckerBoard
  },
  data: function(){
    return{
      xNum:12,
      yNum:9,
      posibility:0.15,
      flag:false
    }
  },
  methods: {
    init(){
      this.$refs.checkerboard.init()
      this.$Message.info({
        content: '重置成功！',
        duration: 3
      });
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-size: 18px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
  margin-left: 30px;
}
.checkerboard{
  float: left;
}
.controls{
  float: left;
  margin-left: 30px;
}
.controls>div{
  margin: 15px 0;
}
</style>
