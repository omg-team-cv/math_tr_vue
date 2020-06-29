<template>
  <div class="training">
    <h1>Hi from vue js!!!</h1>
    <hr>
    <start-screen @onStart="onStart"
    v-if="state == 'start'"></start-screen>
    <question v-else-if="state == 'question'"></question>
    <message v-else-if="state == 'message'"></message>
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
      state: 'start'
    }
  },
  methods: {
    onStart() {
      this.state = 'question'
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