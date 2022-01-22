<template>
    <div id="question-container">
        <div class="rectangle text-white myBorder">
            <div id="sx-rect" class="myBorder"></div>
            <h3>{{question[index].domanda}}</h3>
            <div id="dx-rect" class="myBorder"></div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            
        }
    },
    props: [
        "question",
        "index"
    ]
}
</script>

<style lang="scss" scoped>
#question-container{
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
    height: 140px;
    border-right: none;
    border-left: none;
    padding: 20px;
}

#sx-rect{

    position: absolute;
    left: 0;
    width: 99px;
    height: 99px;
    transform: translate(-50%, 0) rotate(45deg);
    border-top: none;
    border-right: none;
}

#dx-rect{
    position: absolute;
    right: 0;
    width: 99px;
    height: 99px;
    transform: translate(50%, 0) rotate(45deg);
    border-bottom: none;
    border-left: none;

}

@media screen and (max-width: 576px){

    #sx-rect{
        display: none;
    }

    #dx-rect{
        display: none;
    }

    .rectangle{
        min-height: 150px;
        width: 100vw;
        background-color: rgba($color: #000000, $alpha: 0.5);
        border-color: white;
    }

}


</style>