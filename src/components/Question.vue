<template>
  <div class="alert alert-secondary">
        <div class="progress">
      <div class="progress-bar" :style="timeInterval"></div>
    </div>
    <h4>{{timer}} </h4>
    <h3>{{x}} {{randOperator}} {{y}} =?</h3>
    <hr>
    <div class="buttons">
      <button @click="onAnswer(number)"
      v-for="(number, index) in answers"
      :key="index"
       class="btn btn-success">{{number}} </button>
    </div>
  </div>
</template>

<script>
// публичная (екпортируемая) часть модуля
  export default {
    props:['settings'],
    data() {
      return {
        x: mtRand(this.settings.from, this.settings.to),
        y: mtRand(this.settings.from, this.settings.to),
        timer: 5,
        styleDif: null
      }
    },
    computed: {
      timeInterval(){
        return {width: (this.timer / this.styleDif) * 100 + '%'}
      },
      randOperator(){
        let x = this.settings.operator
        x.sort(function(){
         return Math.random() - 0.5;
        })        
        return x[0]
      },
      good(){
        let x;
        switch(this.randOperator){
          case "+":
            x = this.x + this.y;
            break;
          case "-":
            x = this.x - this.y
            break;
        }
        return x
      },
      answers() {
        let variant = [this.good]

        while(variant.length < this.settings.variants){
         let randNum = mtRand(this.good - this.settings.range, this.good + this.settings.range)

         if(variant.indexOf(randNum) === -1){
          variant.push(randNum)
         }
        }
        return variant.sort(function(){
         return Math.random() - 0.5;
        })
      }
    },
    methods: {
      onAnswer(number) {
        if(number == this.good){
          this.$emit('success')
        }else{
          this.$emit('error', `${this.x} ${this.randOperator} ${this.y} = ${this.good}!`)
        }
      },
      changeTimer(){
        if(this.timer > 0){
          this.timer--
        }else{
          this.$emit('error', `${this.x} ${this.randOperator} ${this.y} = ${this.good}!`)
        }
      },
      saveStyleDiff(){
        this.timer = this.settings.timer
        this.styleDif = this.timer
      }
    },
    mounted () {
      let self = this;
      self.saveStyleDiff()
      setInterval(function(){
        self.changeTimer()
      },1000)
    },
  }
  // Приватная часть модуля
  function mtRand(min, max){
    let diff = max - min;
    return Math.floor(Math.random() * (diff + 1)) + min;
  }
</script>

<style scoped>
.alert{
  padding-top: 20px;
  background-color: #eee;
}
.buttons{
  display: flex;
  justify-content: space-around;
}
.btn{
  margin: 20px 0;
}

</style>