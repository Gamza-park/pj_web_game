<template>
    <div>
        <div id="computer" :style="computedStyleObj"></div>
        <div>
            <button @click="onClickButton('Rock')">Rock</button>
            <button @click="onClickButton('Paper')">Paper</button>
            <button @click="onClickButton('Scissors')">Scissors</button>
        </div>
        <div>{{result}}</div>
        <div>Now {{score}} </div>
        <lifecycle-example v-if="true" />
    </div>
</template>

<script>
    const rspCoords = {
        Rock : '0',
        Scissors: '-142px',
        Paper: '-284px'
    };

    const scores = {
        Scissors: 1,
        Rock: 0,
        Paper: -1,
    };

    const computerChoice = (imgCoord) => {
        return Object.entries(rspCoords).find(function (v){
            return v[1] === imgCoord;
        })[0];
    };

    let interval = null;
    export default {
        data(){
            return {
                imgCoord: rspCoords.Rock,
                result: '',
                score: 0,
            };
        },
        computed: {
            computedStyleObj() {
                return {
                    background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`,
                };
            }
        },
        methods: {
            changeHand(){
                interval = setInterval(() => {
                    if(this.imgCoord === rspCoords.Rock){
                        this.imgCoord = rspCoords.Scissors;
                    }
                    else if(this.imgCoord === rspCoords.Scissors){
                        this.imgCoord = rspCoords.Paper;
                    }
                    else if(this.imgCoord === rspCoords.Paper){
                        this.imgCoord = rspCoords.Rock;
                    }
                }, 100);
            },
            onClickButton(choice){
                clearInterval(interval);
                const myScore = scores[choice];
                const cpuScore = scores[computerChoice(this.imgCoord)];
                const diff = myScore - cpuScore;
                if(diff === 0){
                    this.result = "Same";
                }
                else if([-1, 2].includes(diff)){
                    this.result = "Win";
                    this.score += 1;
                }
                else {
                    this.result = "Lose";
                    this.score -= 1;
                }
                setTimeout(() => {
                    this.changeHand();
                }, 1000);
            },

        },
        beforeCreate() {
        },
        created() {
        },
        beforeMount() {
        },
        mounted() {
            this.changeHand();
        },
        beforeUpdate() {
        },
        updated() {
        },
        beforeDestroy() {
            clearInterval(interval);
        },
        destroyed() {
        },
    };
</script>

<style scoped>
    #computer{
        width: 142px;
        height: 200px;
        background-position: 0 0;
    }
</style>