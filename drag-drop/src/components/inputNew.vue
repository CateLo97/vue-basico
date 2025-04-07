<script setup>
import { ref, defineEmits } from 'vue';

    const text = ref("");
    const emit = defineEmits(["onNewItem"]);


    const handleSubmit = () => {
        if (text.value.trim() !== "") {
            emit("onNewItem", text.value);
            text.value = "";
        }

    };

</script>

<template>
<form action="" @submit.prevent="handleSubmit" >
    <input type="text" placeholder="Escribe una tarea" v-model="text">
</form>
</template>
<style scoped>

form {
    margin-bottom: 10px;
}
input {
    width: 100%;
    box-sizing: border-box;
    border-radius: 5px;
    border: 1px solid #ccc;
    margin-bottom: 10px;
    padding: 16px;
}

</style>