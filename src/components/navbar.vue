<template>
    <nav class="navbar" id="navbar-maximized" :class="navMaximized ? 'max' : 'min'">
        <a @click="toggleNav()" href="#" class="menubutton"></a>
        <router-link v-bind:class="'navitem'" to="/">Home</router-link>
        <router-link v-bind:class="'navitem'" to="/page1">Page 1</router-link>
        <router-link v-bind:class="'navitem'" to="/page2">Page 2</router-link>
    </nav>
</template>

<script>
export default {
    data() {
        return {
            navMaximized: true
        }
    },
    methods: {
        toggleNav() {
            if(this.navMaximized){
                this.navMaximized = 0;
                this.$emit('minimizeNav');
            }
            else{
                this.navMaximized = 1;
                this.$emit('maximizeNav');
            }
        },        
    },
    components: {

    },
    emits: [
        'minimizeNav',
        'maximizeNav'
    ]
};
</script>


<style scoped>
nav.navbar{
    position: fixed;
    width: 200px;
    transition: width 500ms;
    height: 100%;
    border: 1px solid black;
    top: 0;
    left: 0;
    overflow-x: hidden;
    background-color: #303030;
    padding-top: 60px;
    font-size: 16pt;
    color:lightgrey;
}
nav.navbar.min{
    width: 55px;
}
nav.navbar.min a.menubutton::after{
    content: "+";
}
nav.navbar.min a.navitem{
    opacity: 0;
    transition: opacity 100ms;
}
nav.navbar.max{
    width: 200px;
}
nav.navbar a.menubutton{
    position: absolute;
    top: 10px;
    font-size: 20pt;
    left: 20px;
    text-decoration: none;
    color:lightgrey;
}
nav.navbar a.menubutton::after{
    content: '×';
}
nav.navbar a.navitem{
    display: block;
    border-bottom: 1px solid black;
    transition: opacity 100ms 300ms;
    padding: 10px 20px;
    text-decoration: none;
    color:lightgrey;
}
nav.navbar :nth-child(2){
    border-top: 1px solid black;
}
</style>