<template>
    <div class="score_modal">
        <div class="score_wrapper">
            <h3>Your Score</h3>

            <h1>{{ correctCount }} / {{ questionLength.length }}</h1>

            <h3>
                {{ rank }}
                <i :class="emoji" aria-role="presentation"></i>
            </h3>

            <button @click="restartQuiz">Restart Quiz</button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['correctCount', 'questionLength'],
    data(){
        return{
            rank:null,
            emoji: null
        }
    },
    methods:{
        restartQuiz(){
            this.$emit('restartQuiz')
        }
    },
    mounted(){
        if(this.correctCount >= 3){
            this.rank = 'Good Job!!'
            this.emoji = 'em em-partying_face'
        }else if(this.correctCount >= 2){
            this.rank = 'Excellent!!'
            this.emoji = 'em em-ok_hand'
        }else if(this.correctCount < 2){
            this.rank = 'Nice try!!'
            this.emoji = 'em em-sweat_smile'
        }
    }
}
</script>

<style scoped>
    /* score modal */
    .score_modal{
        position: fixed;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        background: rgba(0,0,0,0.5);
    }
    .score_wrapper{
        position: absolute;
        top: 50px;
        left: 50%;
        transform: translate(-50%, 0);
        background: #fff;
        width: 100%;
        max-width: 400px;
        border-radius: 8px;
        text-align: center;
        padding: 20px 0;

    }
    .score_wrapper button:nth-child(4){
        padding: 8px 20px;
        border: 1px solid rgb(48, 48, 48);
        outline: none;
        color: #000;
        border-radius: 5px;
        background: #74eb88;
        cursor: pointer;
        font-size: 18px;
        margin-top: 20px;        
        transition: 0.3s;
    }
    .score_wrapper button:nth-child(4):hover{
        background: #9effac;
           border: 1px solid rgb(58, 226, 81);
    }
</style>