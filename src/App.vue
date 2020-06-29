<template>
  <div class="training">
    <h1>Hi from vue js!!!</h1>
    <hr>
    <start-screen 
    @onStart="onStart"
    v-if="state == 'start'"></start-screen>
    <question 
    @error="onQuestError"
    @success="onQuestSuccess"
     v-else-if="state == 'question'"></question>
    <message 
    :type="message.type"
    :text="message.text "
    v-else-if="state == 'message'"></message>
    <result-screen v-else-if="state == 'result'"></result-screen>
    <div v-else>Unknown state</div>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import Message from '@/components/Message.vue';
import Question from '@/components/Question.vue';
import ResultScreen from '@/components/ResultScreen.vue';
import StartScreen from '@/components/StartScreen.vue';
export default {
  components: {
    Message,
    Question,
    ResultScreen,
    StartScreen
  },
  data() {
    return {
      state: 'start',
      message: {
        type: '',
        text: ''
      }
    }
  },
  methods: {
    onStart() {
      this.state = 'question'
    },
    onQuestSuccess(){
      this.state = 'message'
      this.message.text = 'Good Job!'
      this.message.type = 'success'
    },
    onQuestError(msg){
      this.state = 'message'
      this.message.text = msg
      this.message.type = 'warning'
    }
  },
}
</script>

<style scoped>
.training{
  max-width: 700px;
  margin: 20px auto;
}

</style>