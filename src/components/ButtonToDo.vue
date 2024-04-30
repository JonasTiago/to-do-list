<script setup lang="ts">
    import 'primeicons/primeicons.css';
    
    const props = defineProps<{
        icon: string;
        color: string;
        }>();
</script>

<template>
    <button>
        <i :class="props.icon" :style="{ 'color': props.color }"></i>
    </button>
</template>

<style scoped>
    button{
        width: 1.8rem;
        height: 1.8rem;
        border: none;
        background-color: #fff;
        border-radius: 8px;
        cursor: pointer;
        
        &:hover{
            background-color: #ffffffbd;
        }
    }

    
</style>
