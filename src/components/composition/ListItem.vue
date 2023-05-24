<script setup lang="ts">
    export type ListItemT = {
        title: string
        body: string
        id: number
    }
    
    const props = defineProps<ListItemT>()
    const emit = defineEmits(['delete']);

    const handleDelete = () => {
        emit('delete', props.id)
    }
</script>

<template>
    <li>
        <p>{{ title }}</p>
        <p>{{ body }}</p>
        <button @click="handleDelete"> Erase Post </button>
    </li>
</template>