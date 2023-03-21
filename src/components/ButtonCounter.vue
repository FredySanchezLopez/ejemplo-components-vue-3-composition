<script setup>
import { ref } from 'vue';
const counter = ref(0);
const increment  = () => counter.value ++ 
</script>

<template>
    <BUtton @click="increment">Counter {{ counter }}</BUtton>
</template>