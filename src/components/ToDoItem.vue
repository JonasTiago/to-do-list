<script setup lang="ts">
    import { defineProps, ref } from 'vue';

    const props = defineProps<{
        id: number;
        task: string;
        done: boolean;
        toggleDone: Function;
        index: number;
    }>();

    const doneAlt = ref<boolean>(props.done)
    
</script>

<template>
    <div class="item">
        <div>
            <input type="checkbox" 
                v-model="doneAlt"
                @click="toggleDone(index)"
            >
            <p v-if="doneAlt" class="doneTrue">{{ task }}</p>
            <p v-else>{{ task }}</p>
        </div>
        <div>
            <slot />
        </div>
    </div>
</template>

<style scoped>

    .item{
        background-color: #001D3D;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 0.8rem;
        height: 3rem;
        border-radius: 5px;
        margin-top: 1.5rem;

        div{
            display: flex;
            gap: 0.5rem;
        }   
    }

    p{
        font-family: "Ubuntu", sans-serif;
        font-weight: 400;
        font-style: normal;
        font-size: 0.6;
    }

    input[type="checkbox"]{
            width: 1rem;
            height: 1rem;
        }

    .doneTrue{
        text-decoration: line-through;
        color: #fefefe93;
    }
</style>
