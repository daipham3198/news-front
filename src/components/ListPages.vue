<script setup>
import { ref, onMounted } from 'vue'
import BlockPage from './BlockPage.vue'

let pages = ref(null)

onMounted(async () => {
    const response = await fetch('http://127.0.0.1:8000/pages')
    pages.value = await response.json()
})
</script>

<template>
    <div v-if="pages" v-for="page in pages">
        <BlockPage v-if="page" :page="page" />
    </div>
</template>