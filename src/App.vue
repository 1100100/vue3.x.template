<template>
    <router-view />
    <alert :options="dialog"></alert>
    <loading v-if="showLoading" :msg="msg"></loading>
</template>
<script>
import { getCurrentInstance } from "vue";
import alert from "./components/UI/alert.vue";
import loading from "./components/UI/loading.vue";
export default {
    components: { alert, loading },
    data() {
        return {
            showLoading: false,
            dialog: {},
            msg: "加载中...",
        };
    },
    beforeCreate() {
        const app = getCurrentInstance();
        app.appContext.config.globalProperties.$showLoading = (_msg) => {
            if (_msg) this.msg = _msg;
            this.showLoading = true;
        };
        app.appContext.config.globalProperties.$hideLoading = () => {
            this.showLoading = false;
        };
        app.appContext.config.globalProperties.$alert = (arg) => {
            this.dialog = arg;
        };
        app.appContext.config.globalProperties.$http.defaults({
            timeout: 10000,
            $400: (err) => {
                this.$alert({
                    title: "提示",
                    content: err.response.data ? err.response.data : err.response.statusText,
                });
            },
            $401: () => {},
            $finally: () => {
                this.$hideLoading();
            },
        });
    },
};
</script>
<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    font-size: 14px;
}
</style>
