<template>
    <navbar @minimizeNav="navMaximized = 0" @maximizeNav="navMaximized = 1" />
    <div id="rightContent" :style="{ paddingLeft: navMaximized ? 200 + 'px' : 55 + 'px' }">
        <axios-performance :performance="performance" @connectBackendToPLC="connect()"/>
        <router-view :response="response" />
    </div>
</template>

<script>
import axios from 'axios';
import navbar from "./components/navbar.vue";
import axiosPerformance from "./components/axiosPerformance.vue"

// Introduce Axios logging
axios.interceptors.request.use( x => {
    x.meta = x.meta || {}
    x.meta.requestStartedAt = new Date().getTime();
    return x;
});
axios.interceptors.response.use(x => {
        x.responseTime = new Date().getTime() - x.config.meta.requestStartedAt;
        return x;
    },
    // Handle 4xx & 5xx responses
    x => {
        x.responseTime = new Date().getTime() - x.config.meta.requestStartedAt;
        throw x;
    }
)

export default {
    data() {
        return {
            navMaximized: true,
            response : '',
            performance: {
                connected : false,
                connectedToBackend: false,
                connectedToPLC: false,
                ADSreadinterval : 250,
                responseTime : 0
            }
        };
    },
    methods: {
        refresh() {

            // make an axios request to the root which does `readAll` function
            axios
                .get('//localhost:8080/api/controller/', {timeout: 2000})
                .then(
                    // Populate the successful data into the data object
                    response => (this.response = response)
                )
                .then((x) => {
                    // Calculate a rolling average response time
                    this.performance.responseTime = 0.1*x.responseTime + 0.9*this.performance.responseTime;
                    this.performance.connected = true;
                    this.performance.connectedToBackend = true;
                    this.performance.connectedToPLC = true;
                })
                .catch((error) => {
                    
                    if (error.response) {
                        // Request made and server responded
                        this.performance.connected = false;
                        this.performance.connectedToBackend = true;
                        this.performance.connectedToPLC = false;
                    } else if (error.request) {
                        // The request was made but no response was received
                        this.performance.connected = false;
                        this.performance.connectedToBackend = false;
                        this.performance.connectedToPLC = false;
                    } else {
                        // Something happened in setting up the request that triggered an Error
                        console.log('Error', error.message);
                    }

                });
        },
        connect(){
            axios.get('//localhost:8080/api/controller/connect', {timeout: 2000});
        }
    },
    created() {
        this.refresh();
        this.timer = setInterval(this.refresh, this.performance.ADSreadinterval);
    }, 
    components: {
        navbar,
        axiosPerformance
    },
};
</script>

<style>
* {
    font-family: "proxima-nova", sans-serif;
    box-sizing: border-box;
}
div#rightContent {
    transition: padding-left 500ms;
    width: 100%;
    position: relative;
    font-family: monospace;
}
</style>
