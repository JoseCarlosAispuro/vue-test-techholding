<script lang="ts">
    export default {
        props: {
            postId: Number,
            title: String,
            body: String

        },
        methods: {
            handleErasePost () {
                this.$emit('delete', this.postId)
            }
        }
    }
</script>

<template>
    <li>
        <p>{{ title }}</p>
        <p>{{ body }}</p>
        <button @click="handleErasePost">Erase Post</button>
    </li>
</template>