<template>
    <Countdown1 :future-date="date" />
</template>

<script>
import Countdown1 from './components/Countdown.vue';

export default {
    name: 'App',
    components: {
        Countdown1,
    },
    data() {
        return {
            date: '',
        };
    },
    created() {
        const url = new URL(window.location.href);
        this.date = url.searchParams.get('date');
    },
};
</script>
<style lang="scss">
@import 'assets/scss/main.scss';

* {
    box-sizing: border-box;
}
html {
    font-size: 62.5%;
}
body {
    font-family: 'Manrope', sans-serif;
    font-size: 1.6rem;
    text-align: center;
    color: $color-gray-dark-1;
}
</style>
