<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
    question: "Что такое LA?",
    answer: 0,
    options: [
      'Средняя мера нагрузки(load average), отображается в количестве процессов, которые находятся в состоянии выполнения или в состоянии ожидания ресурсов за интервал ',
      'Автоматическая мера нагрузки(load auto), отображается в количестве процессов, которые автоматически распределяется ресурсами компьюера ',
      'Los Angeles',
      'От Франц.- Там',
    ],
    selected: null
  },
  {
    question: "В каких единицах измеряется LA?",
    answer: 1,
    options: [
      'В секундах',
      'В единицах времени',
      'В герцах',
      'В минутах',
    ],
    selected: null
  },
  {
    question: "Что будет если на сервере LA = 100?",
    answer: 2,
    options: [
      'Снижение работоспособности локального компьютера',
      'Сервер перестанет работать и выдаст ошибку 404',
      'Снижение производительности и повышению задержек в обработке запросов ',
      'Ничего не произойдет, т.к чем выше LA, тем быстрее отклик от сервера',
    ],
    selected: null
  },
  {
    question: "Почему при высоких показателях значения LA на сервере может не наблюдаться проблем (консоль ssh отзывается, сервисы работают в обычном режиме)?",
    answer: 0,
    options: [
      'Потому что значения LA могут быть высокими только в течение короткого времени. ',
      'Сервер перестанет работать и выдаст ошибку 404',
      'Потому что значения консо',
      'Потому что значения LA не влияют на отклик сервера',
    ],
    selected: null
  },
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)

const score = computed(() => {
  let value = 0 
  questions.value.map(q => {
    if (q.selected == q.answer){
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(()=>{
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const nextQuestion = () => {
  if(currentQuestion.value < questions.value.length - 1){
    currentQuestion.value++
  } else{
    quizCompleted.value = true
  }
}
</script>

<template>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div> -->
  <!-- <HelloWorld msg="Vite + Vue" /> -->
  <main class="app">
    <h1 class="main-title">The quiz</h1>

    <section class="qiuz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{getCurrentQuestion.question}}</span>
        <span class="score">Score: {{ score }} / {{ questions.length }} </span>
      </div>
      <div class="options">
        <label
        v-for="(option, index) in getCurrentQuestion.options"
            :key="index"
            :class="`option ${
              getCurrentQuestion.selected == index
                ? index == getCurrentQuestion.answer
                  ? 'correct'
                  : 'wrong'
                : ''
            } ${
              getCurrentQuestion != null  && 
              index != getCurrentQuestion.selected
                ? 'disabled'
                : ''
            }`">
            <input 
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :diabled="getCurrentQuestion.selected"
            @change="SetAnswer"
            >
            <span>{{ option }}</span>
        </label>
      </div>
      <button
      @click="nextQuestion"
      :disabled="!getCurrentQuestion.selected"
      class="action-btn"
      >
        {{ 
          getCurrentQuestion.index == questions.length - 1
            ?'Finish'
            : getCurrentQuestion.selected == null
              ? 'Select an option'
              : 'Next'

         }}
      </button>
    </section>
    <section v-else> 
      <h1>You have finished the quiz</h1>
      <p class="score">Score: {{ score }} / {{ questions.length }} </p>
    </section>
  </main>
</template>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
}
.main-title {

}
.qiuz {
  background-color: #382a4b;
  padding: 40px;
  width: 100%;
  max-width: 640px;
  /* margin: 0 auto; */
}
.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1.25rem;
  border-radius: 0.5rem;
}
.quiz-info .question {
  font-size: 1.25rem;
  color: #8f8f8f;
}
.options {
  margin-bottom: 1rem;
}
.quiz-info .score {
  color: #FFF;
  font-size: 1.25rem;
}

.option{
  display: block;
  padding: 1rem;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
  display: flex;
  align-items: start;
}

.option:hover{
  background-color: #2d213f;
}

.option.correct{
  background-color: #2cce7d;
}

.option.wrong{
  background-color: #ff5a5f;
}

.action-btn{
  display: flex;
  flex-direction: column;
  align-items: start;
}

.option:last-of-type{
  margin-bottom: 0;
}

.option.disabled{
  opacity: 0.5;
}

.option input{
  display: none;
}

button{
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}

button:disabled{
  opacity: 0.5;
}

</style>
