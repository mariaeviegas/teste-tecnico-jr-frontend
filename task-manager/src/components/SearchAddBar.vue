<script setup></script>
<template>
    <div class="searchAddBar">
        <input type="text" class="searchAddBar__input" placeholder="Pesquise por título...">
        <button class="searchAddBar__button">Criar tarefa</button>
    </div>
</template>

<style scoped>
    .searchAddBar{
        width: 80%;
        height: fit-content;
        margin: auto;
        background-color: #261F47;
        padding: 1rem 1.5rem;
        border-radius: 1rem;
        display: flex;
        justify-content: space-between;
    }

    .searchAddBar__input {
        width: 40%;
        padding: 0.75rem 1.5rem;
        border: none;
        border-radius: 1.5rem;
        font-size: 1rem;

        &&:focus {
            outline: none;
            background-color: #514983;
            color: #fff;
        }
    }

    .searchAddBar__button {
        width: fit-content;
        padding: 1rem 1.5rem;
        border: none;
        border-radius: 0.75rem;
        background-color: #514983;
        font-size: 1rem;
        color: #fff;
        cursor: pointer;
        transition: 0.5s;

        &&:hover {
            background-color: #514983c9;
        }
    }
</style>