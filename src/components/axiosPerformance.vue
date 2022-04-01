<template>
    <div :class="{connected: performance.connected, disconnected: !performance.connected}" class="performance">Connected: {{ performance.connected }}</div>
    <div v-if="!performance.connectedToBackend" class="performance">Could not connect to backend server.</div>
    <div v-else-if="!performance.connectedToPLC" class="performance">Could not connect to PLC. <button @click="$emit('connectBackendToPLC')">Attempt Connection</button></div>
    <div class="performance">Cycle Time: {{ performance.ADSreadinterval }}ms</div>
    <div class="performance">Response Time: {{ roundResponse }}ms</div>
</template>

<script>
export default {
    data() {
        return {
            data: "Vue"
        };
    },
    emits: ["connectBackendToPLC"],
    props: {
        performance : Object
    },
    methods: {},
    components: {},
    computed: {
        classObject() {
            return {
                active: () => {
                    if(this.performance.connected === true){ return 'connected' }
                    else{ return false }
                }
            }
        },
        roundResponse() {
            if(this.performance.responseTime < 1000){
                return ("000" + Math.round(this.performance.responseTime)).slice(-3);
            }
            else{
                return this.performance.responseTime;
            }
        }
    }
};
</script>

<style scoped>
div.performance{
    float:right;
    clear:both;
    font-size: 8pt;
    color: #707070;
    background-color: #e0e0e0;
    margin: 2px;
    padding: 2px;
}
div.performance.connected{
    background-color: #87e0a9;
    color: black;
}
div.performance.disconnected{
    background-color: #e08787;
    color: black;
}
</style>