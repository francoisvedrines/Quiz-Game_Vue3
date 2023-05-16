<script>
    export default {
        props: ['winCount', 'loseCount']
    }

</script>



<template>

    <section class="score">
        <p>Player <span>{{ this.winCount }}</span> X <span>{{ this.loseCount }}</span> Computer</p>
    </section>

</template>



<style>

section.score {
border-bottom: 1px solid black;
padding: 24px;
font-size: 18px;
}
section.score span {
    padding: 8px;
    font-weight: bold;
    border: 1px solid black;
}

</style>