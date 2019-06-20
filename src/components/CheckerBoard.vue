<template>
  <div class='checkerboard'>
    <div
    v-for="(line,yIndex) in squares"
    :key="yIndex"
    class="line" >
      <div
      v-for="(square,xIndex) in line"
      :key="xIndex"
      class="square"
      @click="select(square,xIndex,yIndex)">
        <div
          v-if="square.selected&&!square.mine"
          class="selected">
          {{square.number}}
        </div>
        <div
          v-else-if="square.selected&&square.mine&&success"
          class="selected">
          <img src='../assets/mine-green.svg' />
        </div>
        <div
          v-else-if="square.selected&&square.mine&&!success"
          class="selected">
          <img src='../assets/mine-red.svg' />
        </div>
        <div
          v-else-if="square.flag"
          class="unselected">
          <img src='../assets/flag.svg' width="100%" height="100%"/>
        </div>
        <div
          v-else
          class="unselected">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CheckerBoard',
  props: {
    xNum: Number,
    yNum: Number,
    posibility: Number,
    flag: Boolean
  },
  data: function(){
    return{
      squares:[],
      mineNum:0,
      selectNum:0,
      success:false,
      end:false
    }
  },
  watch: {
    selectNum: function(){
      if(this.selectNum===this.xNum*this.yNum-this.mineNum){
        this.success=true;
        this.$Message.success({
          content: '成功！',
          duration: 5
        });
        this.showAll();
        this.end=true;
      }
    }
  },
  methods: {
    init(){
      this.squares=[];
      this.mineNum=0;
      this.selectNum=0;
      this.success=false;
      this.end=false;
      for(let i=0;i<this.yNum;i++){
        this.squares.push([]);
        for(let j=0;j<this.xNum;j++){
          this.squares[this.squares.length-1].push({
            mine: Math.random()>1-this.posibility,
            selected: false,
            number: 0,
            flag: false
          })
        }
      }
      for(let i=0;i<this.yNum;i++){
        for(let j=0;j<this.xNum;j++){
          if(this.squares[i][j].mine===true){
            this.mineNum++;
            for(let y=this.confineY(i-1);y<=this.confineY(i+1);y++){
              for(let x=this.confineX(j-1);x<=this.confineX(j+1);x++){
                this.squares[y][x].number++;
              }
            }
          }
        }
      }
    },
    confineX(x){
      if(x===-1){return 0}
      else if(x===this.xNum){return this.xNum-1}
      else{return x}
    },
    confineY(y){
      if(y===-1){return 0}
      else if(y===this.yNum){return this.yNum-1}
      else{return y}
    },
    showAll(){
      for(let i=0;i<this.yNum;i++){
        for(let j=0;j<this.xNum;j++){
          this.squares[i][j].selected=true;
        }
      }
    },
    fail(){
      this.showAll();
      this.end=true;
      this.$Message.error({
        content: '失败！',
        duration: 5
      })
    },
    select(square,x,y){
      if(this.flag){
        square.flag=!square.flag;
      }
      else{
        if(square.mine===true&&this.end===false){
          this.fail();
        }
        else if(square.selected!=true){
          square.selected=true;
          this.selectNum++;
          if(square.number===0){
            this.search(x,y)
          }
        }
      }
    },
    search(x,y){
      if(this.squares[y][x].number===0){
        for(let i=this.confineY(y-1);i<=this.confineY(y+1);i++){
          for(let j=this.confineX(x-1);j<=this.confineX(x+1);j++){
            if((i!=y||j!=x)&&this.squares[i][j].selected!=true){
              this.squares[i][j].selected=true;
              this.selectNum++;
              this.search(j,i);
            }
          }
        }
      }
    }
  },
  created(){
    this.init();
  }
}
</script>

<style scoped>
div.line{
  margin-bottom: 5px;
  height: 50px;
}
div.line>div.square{
  float: left;
  width: 50px;
  height: 50px;
  margin-right: 10px;
  font-size: 30px;
}
div.line>div.square>div.selected{
  width: 50px;
  height: 50px;
  background-color: #eee;
  box-sizing: border-box;
  padding: 5px;
}
div.line>div.square>div.unselected{
  width: 50px;
  height: 50px;
  background-color: #ccc;
}
div.line>div.square>div.unselected:hover{
  background-color: #ddd;
}
</style>
