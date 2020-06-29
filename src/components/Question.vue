<template>
  <div class="alert alert-secondary">
    <h3>{{x}} + {{y}} =?</h3>
    <hr>
    <div class="buttons">
      <button v-for="(answer, index) in answers"
      :key="index"
       class="btn btn-success">{{answer}} </button>
    </div>
  </div>
</template>

<script>
// публичная (екпортируемая) часть модуля
  export default {
    data() {
      return {
        x: mtRand(100, 200),
        y: mtRand(100, 200)
      }
    },
    computed: {
      answers() {
        let good = this.x + this.y
        let variant = [good]

        while(variant.length < 4){
         let randNum = mtRand(good - 20, good + 20)

         if(variant.indexOf(randNum) === -1){
          variant.push(randNum)
         }
        }
        return variant.sort(function(){
          // console.log(Math.random() - 0.5)
         return Math.random() - 0.5;
        })
      }
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

</style>