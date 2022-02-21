<script setup>
import { ref, reactive, computed } from 'vue'
import allQuestions from '../allQuestions'

let qId = ref(0)

const prevQuestion = () => (qId.value > 0 ? (qId.value = qId.value - 1) : 0)
const nextQuestion = () => {
  qId.value < allQuestions.length - 1 ? (qId.value = qId.value + 1) : qId

  const trueIndex = trues.indexOf(handledChoice.item)
  if (trueIndex > -1) {
    trues.splice(trueIndex, 1)
  }

  const falseIndex = falses.indexOf(handledChoice.item)
  if (falseIndex > -1) {
    falses.splice(falseIndex, 1)
  }

  if (handledChoice.item.answer === handledChoice.item.choices[handledChoice.choIceindex]) {
    trues.push(handledChoice.item)
  } else {
    falses.push(handledChoice.item)
  }
}

const trues = reactive([])
const falses = reactive([])

const handledChoice = reactive({
  item: null,
  choIceindex: 0,
  qIndex: 0,
})

const selectedChoice = (item, choIceindex, qIndex) => {
  handledChoice.item = item
  handledChoice.choIceindex = choIceindex
  handledChoice.qIndex = qIndex
}

const showResults = ref(false)

const showResultsFn = computed(() => {
  showResults.value = true
})
</script>

<template>
  <div class="wrapper">
    <div v-if="showResults" class="results">
      <div v-if="trues.length > 0" class="trues">
        <h2>Doğru Cevaplar {{ trues.length }}</h2>
        <ul>
          <li v-for="item in trues" :key="item">
            <p>
              <span>Soru: </span>
              <strong>{{ item.question }}</strong>
            </p>
            <p>
              <span>Cevap: </span>
              <strong>{{ item.answer }}</strong>
            </p>
          </li>
        </ul>
      </div>

      <div v-if="falses.length > 0" class="falses">
        <h2>Yanlış Cevaplar {{ falses.length }}</h2>
        <ul>
          <li v-for="item in falses" :key="item">
            <p>
              <span>Soru: </span>
              <strong>{{ item.question }}</strong>
            </p>
            <p>
              <span>Doğru yanıt: </span>
              <strong>{{ item.answer }}</strong>
            </p>
          </li>
        </ul>
      </div>
    </div>

    <div v-if="!showResults" class="question">
      <div v-for="(item, index) in allQuestions" :key="index">
        <div v-if="index === qId">
          <h2>{{ item.question }}</h2>
          <label v-for="(choice, choiceIndex) in item.choices" :key="choice">
            )
            <input
              @change="selectedChoice(item, choiceIndex, index)"
              type="radio"
              name="a"
              :value="choice"
            />
            <span>{{ choice }}</span>
          </label>
          <div class="btns">
            <button @click="prevQuestion" v-if="qId !== 0">Önceki soru</button>

            <button v-if="qId !== allQuestions.length - 1" @click="nextQuestion">
              <span> Sonraki soru </span>
            </button>

            <button v-if="qId === allQuestions.length - 1" @click="nextQuestion(), showResultsFn()">
              Sonuçları göster
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  border: 1px solid #ccc;
  padding: 1em 2em;
  border-radius: 0.5em;
  counter-reset: my-sec-counter;
}

h2 {
  margin-top: 0;
}

label {
  display: block;
}

label::before {
  counter-increment: my-sec-counter;
  content: counter(my-sec-counter, upper-alpha);
}

.true {
  background-color: green;
}

.btns {
  margin-top: 1em;
  display: flex;
  justify-content: space-between;
}

button {
  background-color: #ccc;
  border: 1px solid #ccc;
  padding: 0.5em 1em;
  border-radius: 0.5em;
  cursor: pointer;
}

button:hover {
  background-color: #ddd;
}
</style>
