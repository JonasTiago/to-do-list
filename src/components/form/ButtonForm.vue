<script setup lang="ts">
import { ref } from 'vue';

      const props = defineProps<{
        name: string;
      }>();
</script>

<template>
    <button type="button" :class="props.name">{{name}}</button>
</template>

<style scoped>
    button{
      border-radius: 100px;
      border: none;
      width: 5.2rem;
      height: 2.6rem;
      cursor: pointer;
      font-family: "Roboto", sans-serif;
      font-weight: bold;
      font-style: normal;
      font-size: 1rem;
    }

    .Edit{
      background-color: #00B4D8;
      color: #fff;
      &:hover{
        background-color: #00b4d88d;
      }
    }

    .Create{
      background-color: #FFC300;
      color: #381E72;
      &:hover{
        background-color: rgba(255, 196, 0, 0.655);
      }
    }

    
</style>
