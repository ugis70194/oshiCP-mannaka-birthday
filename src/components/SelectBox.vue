<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{
    placeholder: string,
    min: number,
    max: number,
}>();

interface Emits {
    (e: 'select', value: number): void
}
const emit = defineEmits<Emits>();

const selectValue = ref(0)
const options = [...Array(props.max-props.min+1)].map((_,i) => i + props.min)
const handler = () => { emit('select', selectValue.value); };

</script>

<template>
    <form @change="handler" class="h-full">
        <select class="w-auto h-full" v-model="selectValue">
            <option disabled value="0">{{ props.placeholder }}</option>
            <option v-for="option in options" :key=option> {{ option }} </option>
        </select>
    </form>
</template>

<style scoped>
</style>
