<template>
    <div class="quiz_wrapper">
      <div class="quiz_header">
        <h3>Quiz Application</h3>
        <div class="timer">
          <span class="time_name">
            {{timeRunning}}
          </span>
          <div class="time">
            {{ seconds }}
          </div>
        </div>
      </div>

      <div class="quiz_body">
        <h2 class="question">{{ getCurrentQuestion.question }}</h2>
        <div class="options">
          <div 
            ref="refOption"
            v-for="(option,index) in getCurrentQuestion.options" :key="option" 
            class="option"
            :class="{
              'correct' : getCurrentQuestion.selected == index && getCurrentQuestion.selected == getCurrentQuestion.answer ,
              'wrong' : getCurrentQuestion.selected == index && getCurrentQuestion.selected != getCurrentQuestion.answer,
              'showcorrect' : getCurrentQuestion.answer == index && getCurrentQuestion.selected != getCurrentQuestion.answer && getCurrentQuestion.selected != null,
              'color' : getCurrentQuestion.selected != null && index != getCurrentQuestion.selected,
              'disabled' : getCurrentQuestion.selected != null && getCurrentQuestion.selected != index
            }
            "
            @click="handleOptionClick(index)">

            {{ option }}
          </div>
        </div>
      </div>

      <div class="quiz_footer">
        <div class="quiz_item">
          {{ currentQuestion + 1 }} of {{ questions.length }}
        </div>

        <button ref="next_btn" @click="handleClickNextBtn" class="next_btn" disabled>
          Next
        </button>
      </div>
    </div>

</template>

<script>

export default {
    props:['questions'],
    methods:{
      handleClickNextBtn(){
        if(this.currentQuestion >= this.questions.length - 1){
          this.scoreModal = !this.scoreModal

          this.$emit('result', this.getScore)
          this.$emit('show', this.scoreModal)
          return

        }else{
           this.setTimeByDiff()
          this.startTimer()
          this.timeRunning = 'Time Left'
          this.currentQuestion ++
        }
    
        this.$refs.next_btn.disabled = 'disabled'
      },
      handleOptionClick(index){
        this.getCurrentQuestion.selected = index
        this.$refs.next_btn.disabled = false,
        clearInterval(this.timer)
      },
      startTimer(){
        this.timer = setInterval(() => {
            this.seconds -= 1  
            if(this.seconds == 0){
                this.$refs.next_btn.disabled = false
                this.timeRunning = 'Time Off'
                this.$refs.refOption.forEach(i => {
                  i.classList.add('disabled')
                });
                clearInterval(this.timer)
            }


        }, 1000)
      },
      setTimeByDiff(){
        this.questions.forEach(i => {
          if(i.difficulty == 'easy'){
              this.seconds = 20
          }else if(i.difficulty == 'medium'){
              this.seconds = 15
          }else if(i.difficulty == 'hard'){
              this.seconds = 7
          }
        })
      }
    },
    data(){
        return{
            currentQuestion: 0,
            score: 0,
            showScore: false,
            timer:null,
            seconds:15,
            timeRunning:'Time Left'
        }
    },
    computed:{
        getCurrentQuestion(){
            return this.questions[this.currentQuestion]
        },
        getScore(){
            this.questions.map(q => {
                if(q.answer == q.selected){
                    this.score++
                }
            })
                return this.score
        }
    },
    mounted() {
        this.setTimeByDiff()
        this.$emit('emitquestions', this.questions)
        this.startTimer()
    }
}
</script>

<style>
.main
{
  padding:0 20px;
}
.main .quiz_wrapper{
box-shadow: rgba(17, 12, 46, 0.15) 0px 48px 100px 0px;
  background: #ffffff;
  max-width: 530px;
  width: 100%;
  border-radius: 8px;
  padding-bottom: 20px;
}
.main .quiz_wrapper .quiz_header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 40px;
  padding: 15px 20px;
box-shadow: 0px 10px 12px -8px rgba(0,0,0,0.10);
-webkit-box-shadow: 0px 10px 12px -8px rgba(0,0,0,0.10);
-moz-box-shadow: 0px 10px 12px -8px rgba(0,0,0,0.10);
}
.main .quiz_wrapper .quiz_header .timer{
  display: flex;
  gap: 10px;
  border: 2px solid rgb(255, 243, 72);
  border-radius: 5px;
  padding: 5px 10px;
  font-size: 19px;
  display: flex;
  align-items: center;
}
.main .quiz_wrapper .quiz_header .timer .time{
  background: #615b22cc;
  padding: 5px;
  border-radius: 5px;
  color: #fff;
}
.quiz_body{
  padding: 0 20px;
}
/* footer */
.main .quiz_wrapper .quiz_footer{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 40px;
    padding: 0 20px;
}
.main .quiz_wrapper .quiz_body .question{
  margin-bottom: 20px;
  font-size: 24px;
}
.option{
  padding: 10px 15px;
  border: 2px solid #999;
  background: #ffffff;
  border-radius: 8px;
  margin-top: 10px;
  transition: 0.2s ease-in;
}
.option.correct{
  background: rgb(209, 253, 209) !important;
  border:2px solid rgb(87, 218, 87) !important;
  color: rgb(87, 218, 87) !important;
}
.option.wrong{
  background: rgb(255, 223, 223) !important;
    border:2px solid rgb(240, 94, 94) !important;
  color: rgb(240, 94, 94) !important;
}
.option.showcorrect{
  background: rgb(209, 253, 209) !important;
  border:2px solid rgb(87, 218, 87) !important;
  color: rgb(87, 218, 87) !important;
}
.option.color{
  pointer-events: none;
  background: rgb(48, 48, 48);
}
.option.disabled{
  pointer-events: none;
  border: 2px solid #999999;
  background-color: #cccccc;
  color: #666666;
}
.option:hover{
  border: 2px solid #f8e323;
  background: rgb(245, 250, 182);
  color: #5c5306;
  cursor: pointer;
}

.next_btn{
  padding: 8px 20px;
  border: 1px solid rgb(48, 48, 48);
  outline: none;
  color: #000;
  border-radius: 5px;
  background: #74eb88;
  cursor: pointer;
  transition: 0.3s ease;
}
.next_btn:hover{
    background: #9effac;
      border: 1px solid rgb(58, 226, 81);
}
.next_btn:disabled,
.next_btn[disabled]{
  border: 1px solid #999999;
  background-color: #cccccc;
  color: #666666;
}

@media screen and (max-width: 550px) {
  .main .quiz_wrapper .quiz_body .question{
    margin-bottom: 20px;
    font-size: 20px;
  }
  .main .quiz_wrapper .quiz_header .timer .time{
    font-size: 18px;
  }
    .main .quiz_wrapper .quiz_header .timer .time_name{
      font-size: 17px;
    }

}
</style>