<script setup lang="ts">
    defineProps<{
            placeholder?: string;
        }>();

    const value = defineModel('value');
</script>


<template>
    <input type="text" v-model="value" :placeholder="placeholder" >
</template>

<style scoped>
   input{
      width: 85%;
      height: 2.5rem;
      margin: 0 1rem 0 0;
      padding: 0.5rem;
      border-radius: 5px;
      font-family: "Roboto", sans-serif;
      font-size: 1rem;
    }
    
</style>
