<template>
    <div>
        <div id="screen" v-bind:class="state" @click="onClickScreen">{{message}}</div>
        <div v-if="result.length">
            <div>avrrage time : {{average}}ms</div>
            <button @click="onReset">Reset</button>
        </div>
    </div>
</template>

<script>
    let startTime = 0;
    let endTime = 0;
    let timeout = null;
    export default {
        data(){
            return{
                result: [],
                state: 'waiting',
                message: 'Please Click This Box',
            }
        },
        computed: {
            average(){
                return this.result.reduce((a, c) => a + c, 0)/this.result.length || 0;
            }
        },
        methods: {
            onReset(){
                this.result = [];
            },
            onClickScreen(){
                if(this.state === 'waiting'){
                    this.state = 'ready';
                    this.message = 'if change color to green, You can click this block ';
                    timeout = setTimeout(() => {
                        this.state = 'now';
                        this.message = 'Now Click!';
                        startTime = new Date();
                    }, Math.floor(Math.random()*1000) + 2000);
                } else if(this.state === 'ready'){
                    clearTimeout(timeout);
                    this.state = 'waiting';
                    this.message = 'please click when block color change to green';
                } else if(this.state === 'now'){
                    endTime = new Date();
                    this.state = 'waiting';
                    this.message = 'Click and Start!'
                    this.result.push(endTime - startTime);
                }
            }
        },
    };
</script>

<style scoped>
    #screen {
        width : 300px;
        height: 200px;
        text-align: center;
        user-select: none;
    }
    #screen.waiting{
        background-color: aqua;
    }
    #screen.ready{
        background-color: red;
        color: white;
    }
    #screen.now{
        background-color: greenyellow;
    }
</style>