<script setup></script>
<template>
    <div class="searchAddBar">
        <input type="text" class="searchAddBar__input" placeholder="Pesquise por título...">
        <button class="searchAddBar__button">Criar tarefa</button>
    </div>
</template>

<style scoped>
.searchAddBar {
    width: 80%;
    height: fit-content;
    margin: 1.5rem auto 0rem;
    background-color: var(--primary-color);
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
        background-color: var(--secondary-color);
        color: white;

        &&::placeholder {
            color: #ffffffa2;
        }
    }
}

.searchAddBar__button {
    width: fit-content;
    padding: 1rem 1.5rem;
    border: 2px solid white;
    border-radius: 0.75rem;
    background-color: transparent;
    font-size: 1rem;
    color: white;
    cursor: pointer;
    transition: 0.1s;

    &&:hover {
        background-color: var(--secondary-color);
        color: white;
        border: 2px solid var(--secondary-color);
    }
}
</style>