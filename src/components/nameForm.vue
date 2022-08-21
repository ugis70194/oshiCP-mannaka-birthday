<script setup lang="ts">
import { ref } from 'vue'

interface Props {
    placeholder: string,
};

const props = defineProps<Props>();

interface Emits {
    (e: 'input', value: string): void
}
const emit = defineEmits<Emits>();

const name = ref("")
const handler = () => { emit('input', name.value); };

</script>

<template>
    <form @change="handler" class="h-full" onsubmit="return false;">
        <input class="h-full" type="text" v-model="name" :placeholder="placeholder" />
    </form>
</template>

<style scoped>
</style>
