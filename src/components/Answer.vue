<template>
    <div>
        <div id="answer-container">
            <div class="rectangle myBorder" :class="[active ? isClicked : '' ]" @click="verifyAnswer">
                <h3>{{answer[index].risposte[n]}}</h3>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            active: false,
            isClicked: 'bg-clicked',
            corretta: 0

        }
    },
    
    props: [
        "answer",
        "n",
        "index",
        "once",
        "endGame",
        "winGame",
        "quizLength"
    ],

    methods: {

        verifyAnswer(){
            let risposta = this.answer[this.index].risposte[this.n]

            if(this.once == false){
                this.isClicked = 'bg-clicked'
                this.active = true;
                this.once = true;
                this.$emit("change-once", this.once);

                //dopo 2 secondi cambia il background della risposta
                setTimeout( ()=> {
                    
                    //se è corretta diventa verde e passa alla prossima
                    if(risposta == this.answer[this.index].esatta){
                        this.isClicked = 'bg-success';
                        this.active = true;
                        
                        if( (this.index + 1) == this.quizLength){

                            setTimeout( () => {
                                this.endGame = true;
                                this.winGame = true;
                                this.$emit('verify-win', {endGame: this.endGame, winGame: this.winGame })
                            }, 700)
                        }

                        setTimeout( () =>{
                            this.isClicked = 'bg-clicked';
                            this.active = false;
                            this.once = false;
                            this.$emit("change-once", this.once);
                            this.$emit("increment-index", this.index);
                        }, 1000)

                    //se è sbagliata diventa rossa e perdi la partita
                    }else{
                        this.isClicked = 'bg-wrong';
                        this.active = true;

                        setTimeout( () => {
                            this.endGame = true;
                            this.winGame = false;
                            this.isClicked = '',
                            this.$emit('verify-win', {endGame: this.endGame, winGame: this.winGame })
                        }, 700)
                    }
                    

                }, 2000)
            }
        },

    },

    created(){
        
    }

}
</script>

<style lang="scss" scoped>
#answer-container{
    display: flex;
    justify-content: center;
}

.myBorder{
    border: 2px solid white;
}

.rectangle{
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 70%;
    height: 80px;
    padding: 15px;
    transition: all 0.3s ease;
    color: white;
    border-radius: 20px;
    &:hover{
        cursor: pointer;
        color: goldenrod;
    }
}

.bg-clicked{
    background-color: goldenrod;
    animation: flashing 1s linear infinite;
}

.bg-success{
    background-color: #089d5a;
}

.bg-wrong{
    background-color: #f62830;
}

@keyframes flashing {
    50% {
        background-color: rgba(218, 165, 32, 0);
    }
}

@media screen and (max-width: 576px){


    .rectangle{
        min-height: 90px;
        min-width: 70%;
        padding: 15px;

        &:hover{
            color: white;
        }

        h3{
            font-size: 1.3rem;
        }
    }

}

@media screen and (min-width: 768px){


    .rectangle{

        h3{
            font-size: 1.2rem;
        }
    }

}

@media screen and (min-width: 995px){


    .rectangle{

        h3{
            font-size: 1.4rem;
        }
    }

}

</style>