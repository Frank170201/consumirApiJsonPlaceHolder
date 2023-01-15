<script setup>
import {ref,computed,onMounted} from 'vue';
import PaginatePost from "./components/PaginatePost.vue";
import BlogPost from "./components/BlogPost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";


const posts = ref([]);
const postXpage=10
const inicio=ref(0)
const fin=ref(postXpage)
const loading=ref(true)

const favorito = ref("");
const cambiarFavorito = (title) => {
  favorito.value = title;
};

const next=() => {
  inicio.value+=+postXpage
  fin.value+=+postXpage
};
const previus=() => {
  inicio.value+=-postXpage
  fin.value+=-postXpage
};

// onMounted(async()=>{
//   try{
//     const res=await fetch('https://jsonplaceholder.typicode.com/posts')
//     posts.value=await res.json()
//   }catch(error){
//     console.error();
//   }finally{
//     loading.value=false
//   }
// })


fetch('https://jsonplaceholder.typicode.com/posts')
  .then(res=>res.json())
  .then(data=>posts.value=data).finally(()=>loading.value=false)
</script>

<template>
  <loading-spinner v-if="loading"/>
  <div class="container" v-else>
    <h1>App</h1>
    <h2>Mis post favorito: {{ favorito }}</h2>

    <paginate-post class="mb-2" @next="next" @prev="previus" :inicio="inicio" :fin="fin" :maxLength="posts.length"></paginate-post>


    <blog-post
      v-for="post1 in posts.slice(inicio,fin)"
      :key="post1.id"
      :title="post1.title"
      :id="post1.id"
      :body="post1.body"
      @cambiarFavoritoNombre="cambiarFavorito"
      class="mb-2"
    ></blog-post>
  </div>
</template>