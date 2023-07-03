<template>
  <div class="main">
    <button ref="start" class="Start" @click="quizStart">Start Quiz</button>

    <div v-if="showDiff" class="diff">
      <h2>Select Difficulty</h2>
      <div class="diff_buttons">
        <button  ref="difficultyButton" @click="clickDifficulty(difficulty)" v-for="difficulty in difficulties" :key="difficulty" :disabled="!showDiff">
          {{difficulty}}
        </button>

      </div>
    </div>

    <QuizComponentVue 
      v-if="showQuiz" 
      :questions="filteredDifficulty" 
      @result="result" @show="show" 
      @emitquestions="emitquestions"
    />
    
    <ScoreComponentVue 
      v-if="scoreModal" 
      :correctCount="correctCount" 
      :questionLength="getQuestion" 
      @restartQuiz="restartQuiz"
    />

  </div>


</template>

<script>
import QuizComponentVue from './components/QuizComponent.vue'
import ScoreComponentVue from './components/ScoreComponent.vue'


export default {
  name: 'App',
  components: { ScoreComponentVue, QuizComponentVue },
  data(){
    return{
      showDiff:false,
      showQuiz:false,
      scoreModal:false,
      correctCount: null,
      difficulties:['easy','medium','hard'],
      selectedDifficulty:null,
      questions:[
          { 
          difficulty: 'easy',
          question: 'How many stars in the galaxy',
          answer: 2,
          options:[
              '2',
              '9',
              '8',
              '10',
          ],
          selected:null
          },
          { 
          difficulty: 'easy',
          question: 'What is Vue Js?',
          answer: 1,
          options:[
              'A pie',
              'A Javascript Framework',
              'A Refrigirator',
              'An Internet Connection',
          ],
          selected:null
          },
          { 
          difficulty: 'easy',
          question: 'When is Luke Birthday?',
          answer: 3,
          options:[
              'April 21, 2000',
              'January 20, 2001',
              'June 15, 2002',
              'August 25, 2000',
          ],
          selected:null
          },
          { 
          difficulty: 'easy',
          question: 'What is "cynophobia"?',
          answer: 0,
          options:[
              'Fear of dogs',
              'Fear of cats',
              'Fear of crocodile',
              'Fear of bats',
          ],
          selected:null
          },
          // medium
          { 
          difficulty: 'medium',
          question: 'What does “www” stand for in a website browser?',
          answer: 3,
          options:[
              'World Wide Wwagon',
              'World Wide Wings',
              'World War Web',
              'World Wide Web',
          ],
          selected:null
          },
          { 
          difficulty: 'medium',
          question: 'How long is an Olympic swimming pool (in meters)?',
          answer: 0,
          options:[
              '50 meters',
              '30 meters',
              '60 meters',
              '55 meters',
          ],
          selected:null
          },
          { 
          difficulty: 'medium',
          question: 'Which country do cities of Perth, Adelade & Brisbane belong to?',
          answer: 1,
          options:[
              'India',
              'Australia',
              'US',
              'Philippines',
          ],
          selected:null
          },
          // hard
          { 
          difficulty: 'hard',
          question: 'What is the name of the biggest technology company in South Korea??',
          answer: 2,
          options:[
              'Cherry Mobile',
              'Iphone',
              'Samsung',
              'Xiaomi',
          ],
          selected:null
          },
          { 
          difficulty: 'hard',
          question: 'What is the rarest M&M color?',
          answer: 3,
          options:[
              'Red',
              'Blue',
              'Pink',
              'Brown',
          ],
          selected:null
          },
          { 
          difficulty: 'hard',
          question: 'What was the first soft drink in space?',
          answer: 0,
          options:[
              'Coca Cola',
              'Sprite',
              'Royal',
              'C2',
          ],
          selected:null
          },
      ],
      // getQuestion:[]
    }
  },
  methods:{
    result(resultQuiz){
      this.correctCount = resultQuiz
    },
    show(res){
      this.scoreModal = res
    },
    emitquestions(ques){
        this.getQuestion = ques
    },

    // start quiz
    quizStart(){
      this.showDiff = !this.showDiff
      this.$refs.start.style.display="none"
    },
    // select difficuulty
    clickDifficulty(diff){
      this.selectedDifficulty = diff
      this.showDiff = !this.showDiff
      this.showQuiz = !this.showQuiz
    },
    // restart quiz
    restartQuiz(){
        this.filteredDifficulty.forEach(i => {
          i.selected = null
        })

        this.$refs.start.style.display="block"
        this.showQuiz = !this.showQuiz
        this.scoreModal = !this.scoreModal
        
    }
  },
  computed:{
    filteredDifficulty(){
      return this.questions.filter((question) => question.difficulty == this.selectedDifficulty)
    }
  }
}
</script>

<style>
html{
  font-family: 'Poppins', sans-serif;
}
*{
  font-family: 'Poppins', sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border;
}

.main{
  background: rgb(223, 230, 139);
  display: grid;
  place-items: center;
  height: 100vh;
}

.Start{
  width: 140px;
  height: 55px;
  border: 2px solid #f8e323;
  background: rgb(245, 250, 182);
  color: #252525;
  cursor: pointer;
  font-size: 22px;
  border-radius: 8px;
  transition: 0.3s ease;
}
.Start:hover{
   background: #f8e323;
}

/* difficulty */
.diff h2{
  text-align: center;
  font-size: 32px;
  color: rgb(85, 82, 53);
  margin-bottom: 20px;
}
.diff button{
  padding:8px 20px ;
  outline: none;
  font-size: 20px;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.3s ease;
}
.diff button:nth-child(1){
  border:2px solid #53da53;
  background: #ddffdd;
  color: #53da53;
}
.diff button:nth-child(1):hover{
    background: #8bc98b;
    color: #fff;
}
.diff button:nth-child(2){
    border:2px solid #d3c71c;
  background: #f9ffc1;
  color: #c2b71a;
}
.diff button:nth-child(2):hover{
    background: #828848;
  color: #ffffff;
}
.diff button:nth-child(3){
  border:2px solid #303030;
  background: #b3b3b3;
  color: #1d1d1d;
}
.diff button:nth-child(3):hover{
  background: #616161;

  color: #fff;
}
.diff .diff_buttons{
  display: flex;
  gap: 10px;
}
.diff_buttons:disabled,
.diff_buttons[disabled]{
  border: 1px solid #999999;
  background-color: #cccccc;
  color: #666666;
}
.diff_buttons{
  display: flex;
  flex-wrap: wrap;
}
</style>
