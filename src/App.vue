<script setup>


</script>

<template>
<div>    
    <h1>Questions</h1>

    <input type="radio" name="options" value="True">
    <label>True</label><br>

    <input type="radio" name="options" value="False">
    <label>False</label><br>

    <button class="send" type="button">Send</button>
</div>

</template>

<style>
#app {
    font-family: Arial, Helvetica, sans-serif;
    text-align: center;
    color: black;
    margin: 60px auto;
    max-width: 960px;
}

input[type=radio]{
    margin: 12px 4px;
}

button.send{
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: blue;
    border: 1px solid blue;
    cursor: pointer;
}

</style>
