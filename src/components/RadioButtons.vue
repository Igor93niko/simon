<template>
  <div class="row">
    <h3>Выбери уровень сложности:</h3>
    <div class="line"> 
      <input type="radio" id="hard" value="hard" name="level" @change="$emit('changeLevel','hard')"/>
      <label for="hard">Сложный</label>
    </div>
     <div class="line">
      <input type="radio" id="medium" value="medium" checked name="level" @change="$emit('changeLevel','medium')"/>
      <label for="medium">Средний</label>
    </div>
     <div class="line">
      <input type="radio" id="easy" value="easy" name="level" @change="$emit('changeLevel','easy')"/>
      <label for="easy">Легкий</label>
    </div>
  </div>
</template>

<script>

export default{
  name:'RadioButtons'
}
</script>
<style scoped>

.line{

}
</style>