<template>
    <div class="page">{{ response.data }} <br /></div>
</template>

<script>
export default {
    data() {
        return {
            data: "Vue"
        };
    },
    props: [
        'response'
    ],
    methods: {},
    components: {},
};
</script>

<style scoped>
</style>