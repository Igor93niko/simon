<template>
  <div id="app">
    <div class="container" v-if="!this.lose">
      <div class="row">
          <h1>
            Simon
          </h1>
      </div>
      <hr>
      <div class="row">
        <div class="col">
          <div @click="btnHandler" :class="{ button: true, first: true, active: isActive[0] }" id='0'></div>
          <div @click="btnHandler" :class="{ button: true, second: true, active: isActive[1] }" id='1'></div>
        </div>
        <div class="col">
          <div @click="btnHandler" :class="{ button: true, third: true, active: isActive[2] }" id='2'></div>
          <div @click="btnHandler" :class="{ button: true, fourth: true, active: isActive[3] }" id='3'></div>
        </div>
        <button @click="startGame"  v-if="isChangeLevel">Начать игру!</button>
        <RadioButtons
        @changeLevel="changeLevel" v-if="isChangeLevel"
        />
      </div>
    </div>
    <div class="lose" v-else>
      <h1> Вы проиграли</h1>
      <button @click="clickHandler">ОК</button>
    </div>
  </div>
</template> 

<script>

import audio1 from '@/audio/audio1.wav';
import audio2 from '@/audio/audio2.wav';
import audio3 from '@/audio/audio3.wav';
import audio4 from '@/audio/audio4.wav';
import RadioButtons from '@/components/RadioButtons.vue';
export default {
  name: 'App',
  components: {
    RadioButtons
  },
  data: function(){
    return({
      lose:false,
      isActive:[false,false,false,false],
      isChangeLevel:true,
      steps:[],
      level:1,
      nowLevel:0,
      block:true,
      time:1000,
      audio:[audio1,audio2,audio3,audio4],
      numClick:0
    })
  },
  methods:{
    clickHandler: function(){
      this.lose = false;
    },
    changeLevel: function(level){
      switch(level){
        case 'easy':{
          this.time = 1500;
          break;
        }
        case 'hard':{
          this.time = 400;
          break;
        }
        case 'medium':{
          this.time = 1000;
          break;
        }
        default: this.time = 1000;
      }
    },
    rndNum:function(){
      const num = Math.floor(Math.random()*4);
      return num;
    },
     btnHandler:function(event){
      if (!this.block){
      const id = event.target.id;
      
      if (this.steps[this.numClick]!==+id){
        this.lose = true;
          this.cleare(); 
          this.steps = [];
          this.time = 1000;
          this.isChangeLevel = true; 
          this.level = 1;
      }
      else{
        this.changeColor(+id);
        this.numClick++;
      }
    }
    },
    start:function(num){
      setTimeout(()=>{
        if (this.steps[this.nowLevel]!=undefined){
          this.changeColor(this.steps[this.nowLevel]);
        }
        else{
          this.steps.push(num);
          this.changeColor(num);
        }
       },this.time/2);
    },
    cleare:function(){
      this.nowLevel=0;
      this.block=true;
      this.numClick=0;
    },
    changeColor:function(num){
      const before = this.isActive.slice(0,num);
      const after = this.isActive.slice(num+1);
      this.isActive = [...before, true, ...after];
      this.playAudio(num);
      setTimeout(()=>{
        this.nowLevel++;
        this.isActive = [...before, false, ...after];
        if (this.block){
        if (this.nowLevel<this.level){
          this.start(this.rndNum());
        }
        else{
          this.block=false;
        }
        }
        else{
          if (this.numClick===this.steps.length){
          this.cleare();
          this.level ++; 
          setTimeout(()=>{
            this.start(this.steps[0]);
          },500);
         }     
        }
      },this.time/2);
    },
    playAudio(num){
      const audio = new Audio(this.audio[num])
      audio.play();
    },
    startGame(){
      this.isChangeLevel = false;
      this.start(this.rndNum());
    }
  }
}
</script>

<style >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.row{
  width: 100%;
}
.col{
  display: flex;
  justify-content: center;
  width: 100%;
}
.button{
  margin: 5px;
  border-radius: 50%;
  width: 33%;
}
.button::after{
  content:'';
  padding-top:100%;
  display: block;
}
.button:hover{
  cursor: pointer;
}
.first{
  background-color: red;
  box-sizing: border-box;
}
.second{
  background-color: yellow;
  box-sizing: border-box;
}
.third{
  background-color: green;
  box-sizing: border-box;
}
.fourth{
  background-color: blue;
  box-sizing: border-box;

}

.first.active{
  background-color: white;
  border: solid 3px red;
}

.second.active{
  background-color: white;
  border: solid 3px yellow;
}

.third.active{
  background-color: white;
  border: solid 3px green;
}
.fourth.active{
  background-color: white;
  border: solid 3px blue;
}

button{
  padding: 10px 25px;
  font-size: 16px;
  color: white;
  font-weight: bold;
  background-color: blue;
  border-radius: 10px;
  border: white solid 2px;
  box-shadow: blue 0px 2px 2px 0px;
}
button:hover{
  cursor: pointer;
  color: blue;
  border-color: blue;
  background-color: white;
}
.lose{
  position: absolute;
  top: 5em;
  width: 100%;
  height: 200px;
  background-color: white;
}
</style>
