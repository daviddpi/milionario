<template>
    <div>
      <audio id="myAudio">
        <source src="../../public/audio.mp3" type="audio/mpeg">
      </audio>

      <!-- sezione indicazione vittoria o sconfitta -->
      <div v-if="endGame" class="end-game">
        <div>
          <h4>{{winGame ? 'Hai vinto!' : 'Hai perso'}}</h4>
        </div>
        <div>
          <h5>
            Premio: <span v-if="index > 0">{{winGame ? quiz[this.index].premio : quiz[this.index - 1].premio}} &euro;</span>
            <span v-else>0 &euro;</span>
          </h5>
        </div>
        <button @click="reStartGame" class="btn" :class="[winGame ? 'btn-primary' : 'btn-danger' ]">{{winGame ? 'Gioca di nuovo' : 'Riprova '}}</button>
      </div>

      <!-- sezione indicazione premio -->
      
      <div v-if="showAward" class="award d-none d-sm-none d-md-block">
        <div v-for="item in quiz.slice().reverse()" :key="item">
          <h6>Premio: {{item.premio}}&euro;</h6>
        </div>
      </div>
        
      <div class="icon-award d-block d-md-none d-lg-none">
        <i class="fas fa-bars" @click="showSmAward"></i>
      </div>

      <div v-show="showAwardSm" class="award-sm d-md-none d-lg-none">
        <i @click="showSmAward" class="fas fa-times fa-2x"></i>
        <div v-for="element in quiz.slice().reverse()" :key="element.premio">
          <h5>Premio: {{element.premio}}&euro;</h5>
        </div>
      </div>
      

      <div class="container">
          <div class="row" v-if="pressPlay">
              <div class="col-12">
                  <div class="start-animation">
                      <div id="logo">
                          <img src="../../public/logo.png" alt="Logo">
                      </div>
                      <div>
                          <button type="button" class="btn btn-outline-dark myBtn" @click="startGame">Gioca</button>
                      </div>
                  </div>
              </div>
          </div>
          <div class="row" v-else>
              <div class="col-12 mb-5">
                  <div id="logo">
                      <img src="../../public/logo.png" alt="Logo">
                  </div>
              </div>
              <div class="col-12 mb-3">
                  <Question :question="quiz" :index="index" clasS="flicker-in-1"></Question> 
              </div>
              <div v-for="n in 4" :key="n" class="col-md-6 col-12 g-4 pb-3">
                  <Answer :answer="quiz" :n="n - 1" :quizLength="quizLength" :index="index" :once="once" :endGame="endGame" :winGame="winGame" @increment-index="incrementIndex"
                  @change-once="changeOnce" @verify-win="verifyWin" class="flicker-in-1"></Answer> 
              </div>
          </div>
      </div>
    </div>
</template>

<script>

import Question from './Question.vue'
import Answer from './Answer.vue'
import quiz from '../data/milionaire.json'

export default {
    data(){
        return{
            quiz,
            index: 0,
            once: false,
            pressPlay: true,
            endGame: false,
            winGame: '',
            quizLength: quiz.length,
            showAward: false,
            showAwardSm: false
        }
    },

    components: {
        Question,
        Answer,

    },

    methods: {
        incrementIndex(){
            this.index++;

            this.addBg();
            this.addBgSm();


        },

        changeOnce(once){
            this.once = once;
        },

        addBgSm(){
            let listAwardSm = document.querySelector('.award-sm');
            let awardChildSm = Array.from(listAwardSm.childNodes);
            awardChildSm.reverse();
            awardChildSm[this.index - 1].classList.add('bg-gold');
        },

        addBg(){
            let listAward = document.querySelector('.award');
            let awardChild = Array.from(listAward.childNodes);

            awardChild.reverse();

            awardChild[this.index - 1].classList.add('bg-gold');

        },

        removeBg(){
            let listAward = document.querySelector('.award');
            let awardChild = Array.from(listAward.childNodes);
            awardChild.reverse();
            awardChild.forEach(element => {
              element.classList.remove('bg-gold');
            });
        },

        removeBgSm(){
          let listAwardSm = document.querySelector('.award-sm');
          let awardChildSm = Array.from(listAwardSm.childNodes);
          awardChildSm.reverse();
          awardChildSm.forEach(element => {
            element.classList.remove('bg-gold');
          });
        },

        verifyWin(show){
          this.endGame = show.endGame;
          this.winGame = show.winGame;
          console.log(show.endGame, show.winGame)
        },
        
        shuffle(a) {
            for (let i = a.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [a[i], a[j]] = [a[j], a[i]];
            }
            return a;
        },

        startGame(){
            let song = document.getElementById("myAudio");
            song.play();
            this.pressPlay = false;
            this.endGame = false;
            this.winGame = '';
            this.index = 0;
            this.once = false;
            this.showAward = true;
            this.removeBg();
            this.removeBgSm();

        },

        reStartGame(){
            this.pressPlay = false;
            this.endGame = false;
            this.winGame = '';
            this.index = 0;
            this.once = false;
            this.showAward = true;
            this.removeBg();
            this.removeBgSm();

        },

        showSmAward(){
          this.showAwardSm = !this.showAwardSm;
          console.log(this.showAwardSm);
        }

    },

    created(){
        let arr = this.quiz
        arr.forEach(element => {
            element.risposte
            this.shuffle(element.risposte)
        });
    }
}
</script>

<style lang='scss' scoped>

.end-game{
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 350px;
  height: 250px;
  background-color: rgba($color: #000000, $alpha: .75);
  border-radius: 25px;
  color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  div{
    padding: 10px;
  }
  z-index: 2;
  animation: slide-in-blurred-top 0.6s;
}

@keyframes slide-in-blurred-top {
  0% {
    top: 0%;
    filter: blur(40px);
    opacity: 0;
  }
  100% {
    filter: blur(0);
    opacity: 1;
  }
}

.start-animation{
    display: flex;
    flex-direction: column;
    align-items: center;
    animation: bounce-in-top 1.1s both;
}

@keyframes bounce-in-top {
  0% {
    transform: translateY(-500px);
    animation-timing-function: ease-in;
    opacity: 0;
  }
  38% {
    transform: translateY(0);
    animation-timing-function: ease-out;
    opacity: 1;
  }
  55% {
    transform: translateY(-65px);
    animation-timing-function: ease-in;
  }
  72% {
    transform: translateY(0);
    animation-timing-function: ease-out;
  }
  81% {
    transform: translateY(-28px);
    animation-timing-function: ease-in;
  }
  90% {
    transform: translateY(0);
    animation-timing-function: ease-out;
  }
  95% {
    transform: translateY(-8px);
    animation-timing-function: ease-in;
  }
  100% {
    transform: translateY(0);
    animation-timing-function: ease-out;
  }
}

.myBtn{
    border-color: white;
    color: white;
    padding: 15px 45px;
    font-size: 1.2rem;
    &:hover{
        background-color: goldenrod;
    }
}

#logo{
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 25px;
    padding-top: 50px;
}

img{
    width: 225px;
    border-radius: 50%;
    box-shadow:  20px 20px 83px #9c1277,
             -20px -20px 83px #d218a1;
}

.flicker-in-1 {
	animation: flicker-in-1 2s linear both;
}

@keyframes flicker-in-1 {
  0% {
    opacity: 0;
  }
  10% {
    opacity: 0;
  }
  10.1% {
    opacity: 1;
  }
  10.2% {
    opacity: 0;
  }
  20% {
    opacity: 0;
  }
  20.1% {
    opacity: 1;
  }
  20.6% {
    opacity: 0;
  }
  30% {
    opacity: 0;
  }
  30.1% {
    opacity: 1;
  }
  30.5% {
    opacity: 1;
  }
  30.6% {
    opacity: 0;
  }
  45% {
    opacity: 0;
  }
  45.1% {
    opacity: 1;
  }
  50% {
    opacity: 1;
  }
  55% {
    opacity: 1;
  }
  55.1% {
    opacity: 0;
  }
  57% {
    opacity: 0;
  }
  57.1% {
    opacity: 1;
  }
  60% {
    opacity: 1;
  }
  60.1% {
    opacity: 0;
  }
  65% {
    opacity: 0;
  }
  65.1% {
    opacity: 1;
  }
  75% {
    opacity: 1;
  }
  75.1% {
    opacity: 0;
  }
  77% {
    opacity: 0;
  }
  77.1% {
    opacity: 1;
  }
  85% {
    opacity: 1;
  }
  85.1% {
    opacity: 0;
  }
  86% {
    opacity: 0;
  }
  86.1% {
    opacity: 1;
  }
  100% {
    opacity: 1;
  }
}

.award{
  position: absolute;
  right: 2%;
  top: 5%;
  background-color: rgba($color: #000000, $alpha: .75);
  width: max-content;
  height: max-content;
  padding: 30px;
  color: white;
  border-radius: 20px;
  animation: tilt-in-tr 0.65s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
}

.award-sm{
  position: absolute;
  top: 70%;
  left: 50%;
  transform: translate(-50%, -70%);
  background-color: rgba($color: #000000, $alpha: .75);
  width: max-content;
  height: max-content;
  padding: 10px 40px;
  color: white;
  border-radius: 20px;
  animation: tilt-in-tr-2 0.65s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
  z-index: 100;
}

@keyframes tilt-in-tr {
  0% {
    transform: rotateY(-35deg) rotateX(20deg) translate(250px, -250px) skew(-12deg, -15deg);
    opacity: 0;
  }
  100% {
    transform: rotateY(0) rotateX(0deg) translate(0, 0) skew(0deg, 0deg);
    opacity: 1;
  }
}

@keyframes tilt-in-tr-2 {
  0% {
    transform: rotateY(-35deg) rotateX(20deg) skew(-12deg, -15deg) translate(0,-100%);
    opacity: 0;
  }
  100% {
    transform: rotateY(0) rotateX(0deg) skew(0deg, 0deg) translate(-50%, -70%);
    opacity: 1;
  }
}

.bg-gold{
  background-color: goldenrod;
  padding: 0 10px;
  border-radius: 25px;
}

.icon-award{
  position: absolute;
  right: 5%;
  top: 5%;
  z-index: 10;
  .fa-bars{
    color: white;
    font-size: 2.3rem;
  }
}

.fa-times{
  padding-left: 100%;
  padding-bottom: 10px;
}
</style>