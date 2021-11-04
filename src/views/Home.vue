<template>
    <div class="home"></div>
</template>

<script>
export default {
    name: "Home",
    components: {},
    mounted() {
        this.$alert({ title: "2", content: "1" });
        this.$http.get("/api");
    },
    methods: {},
};
</script>
