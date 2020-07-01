<template>
  <div class="training">
    <h1>Hi from vue js!!! Level: {{level + 1}}</h1>
    <hr />
    <div class="progress">
      <div class="progress-bar" :style="progresStyles"></div>
    </div>
    <div class="box">
      <transition name="flip" mode="out-in">
        <start-screen @onStart="onStart" v-if="state == 'start'"></start-screen>
        <question
          :settings="levels[level]"
          @error="onQuestError"
          @success="onQuestSuccess"
          v-else-if="state == 'question'"
        ></question>
        <message
          @onNext="onNext"
          :type="message.type"
          :text="message.text "
          v-else-if="state == 'message'"
        ></message>
        <result-screen
          @repeat="onStart"
          @nextLevel="nextLevel"
          :stats="stats"
          v-else-if="state == 'result'"
        ></result-screen>
        <div v-else>Unknown state</div>
      </transition>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.css";
import Message from "@/components/Message.vue";
import Question from "@/components/Question.vue";
import ResultScreen from "@/components/ResultScreen.vue";
import StartScreen from "@/components/StartScreen.vue";
export default {
  components: {
    Message,
    Question,
    ResultScreen,
    StartScreen
  },
  data() {
    return {
      state: "start",
      stats: {
        success: 0,
        error: 0
      },
      message: {
        type: "",
        text: ""
      },
      // questMax: 2,
      level: 0,
      levels: [
        {
          operator: ["+","-"],
          timer: 10,
          questMax: 4,
          variants: 2,
          from: 1,
          to: 30,
          range: 10
        },
        {
          operator: ["+","-"],
          timer: 10,
          questMax: 8,
          variants: 4,
          from: 1,
          to: 100,
          range: 30
        },
        {
          operator: ["+","-"],
          timer: 15,
          questMax: 10,
          variants: 6,
          from: 1,
          to: 500,
          range: 80
        }
      ]
    };
  },
  computed: {
    questMax(){
      return this.levels[this.level].questMax
    },
    questDone() {
      return this.stats.success + this.stats.error;
    },
    progresStyles() {
      let bg = 'red'
      if ((this.questDone / this.questMax) * 100 > 50){
        bg = 'green'
      }
      return  [{
        width: (this.questDone / this.questMax) * 100 + "%"
      }
      // ,{
      //   background: bg
      // }
      ];
    }
  },
  methods: {
    onStart() {
      this.state = "question";
      this.stats.success = 0;
      this.stats.error = 0;
    },
    onQuestSuccess() {
      this.state = "message";
      this.message.text = "Good Job!";
      this.message.type = "success";
      this.stats.success++;
    },
    onQuestError(msg) {
      this.state = "message";
      this.message.text = msg;
      this.message.type = "warning";
      this.stats.error++;
    },
    onNext() {
      if (this.questDone < this.questMax) {
        this.state = "question";
      } else {
        this.state = "result";
      }
    },
    nextLevel() {
      if (this.level < this.levels.length - 1){
        this.level++;
        this.onStart();
      }else{
        this.stats.success = 0;
        this.stats.error = 0;
        this.level = 0;
        this.state = 'start'
      };
      
    }
  }
};
</script>

<style scoped>
.training {
  max-width: 700px;
  margin: 20px auto;
}
.box {
  margin: 10px 0;
}
.progress-bar {
  transition: 0.6s;
}

.flip-enter {
}
.flip-enter-active {
  animation: flipInX 0.3s linear;
}
.flip-leave {
}
.flip-leave-active {
  animation: flipOutX 0.3s linear;
}
@keyframes flipInX {
  from {
    transform: rotateX(90deg);
  }
  to {
    transform: rotateX(0deg);
  }
}
@keyframes flipOutX {
  from {
    transform: rotateX(0deg);
  }
  to {
    transform: rotateX(90deg);
  }
}
</style>