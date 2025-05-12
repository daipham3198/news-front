<template>
    <div class="header">
      <span>{{ page.title }}</span>
      <button class="reload-btn" @click="fetchNews">⟳</button>
    </div>
    <div class="trending">
      <ul class="trending-list">
        <li v-for="ns in news" :key="ns.title" class="trending-item">
          <a
            class="topic"
            :href="ns.url.includes('http') ? ns.url : page.base_url + ns.url"
            target="_blank"
          >
            <h4>{{ decodeHtml(ns.title) }}</h4>
          </a>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  
  const props = defineProps({
    page: {
      type: Object,
      required: true
    }
  })
  
  let news = ref(null)
  
  function decodeHtml(html) {
    const txt = document.createElement("textarea");
    txt.innerHTML = html;
    return txt.value;
  }
  
  async function fetchNews() {
    const url = `http://127.0.0.1:8000/pages/${props.page.title.toLowerCase()}`
    const response = await fetch(url)
    news.value = await response.json()
  }
  
  onMounted(fetchNews)
  </script>
  
  <style scoped>
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 18px;
    margin-bottom: 10px;
  }
  
  .reload-btn {
    background-color: #f0f0f0;
    border: none;
    cursor: pointer;
    font-size: 18px;
    padding: 4px 8px;
    border-radius: 4px;
    transition: background-color 0.2s;
  }
  
  .reload-btn:hover {
    background-color: #ddd;
  }
  
  .trending-list {
    list-style: none;
    padding: 0;
  }
  
  .trending-item {
    margin-bottom: 8px;
  }
  
  .topic {
    text-decoration: none;
    color: #333;
  }
  
  .topic:hover {
    text-decoration: underline;
  }
  </style>
  