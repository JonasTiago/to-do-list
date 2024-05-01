<script setup lang="ts">
    import { ref, defineProps } from 'vue';

    const props = defineProps<{
        id: number;
        task: string;
        done: boolean;
        selectEdit: boolean;
    }>();

    const alterTask = ref<{
        done:boolean
    }>({
        done: props.done
    });

    const emit = defineEmits(['change']);
    
    const emitirEvento = () => {
        emit('change');
    };

</script>

<template>
    <div class="item">
        <div>
            <input 
                type="checkbox" 
                v-model="alterTask.done"
                @click="emitirEvento"
            >
            <p :class="{ doneTrue : alterTask.done }">{{ task }}</p>
        </div>
        <div>
            <slot ></slot>
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
            align-items: center;
            gap: 0.5rem;
        }   
    }

    p{  
        font-family: "Ubuntu", sans-serif;
        font-weight: 400;
        font-style: normal;
        font-size: 0.6;
        line-height: 1.5;
        text-overflow: ellipsis;
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
