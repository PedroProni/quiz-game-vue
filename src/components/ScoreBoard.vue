<template>
    <section class="score">
        Score Board Player <span>{{ this.winCount }}</span> x <span> {{ this.loseCount }}</span> Computer
    </section>
</template>

<script>
export default {
    props: {
        winCount: {
            type: Number,
            default: 0
        },
        loseCount: {
            type: Number,
            default: 0
        }
    }
}
</script>

<style lang="scss">
section.score {
    border-bottom: 1px solid black;
    padding: 24px;
    font-size: 18px;
}

section.score span {
    padding: 8px;
    font-size: bold;
    border: 1px solid black;
}
</style>