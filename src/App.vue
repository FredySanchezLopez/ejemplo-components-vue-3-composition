<script setup>
import { ref, computed, onMounted } from 'vue';

import PaginatePost from './components/PaginatePost.vue';
import BlogPost from './components/BlogPost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue'


const posts = ref([]);
const postxpage = 10;
const inicio = ref(0);
const fin = ref(postxpage);
const loading = ref(true);

const favorito = ref("");

const cambiarFavorito = (title) => {
  favorito.value = title;  
};

const next = () => {
  inicio.value = inicio.value + postxpage
  fin .value = fin.value + postxpage
}

const prev = () => {
  //inicio.value = inicio.value - postxpage
  inicio.value += - postxpage
  fin.value += - postxpage
}

// Con esta opción podemos hgacer uso del onMounted para obtener los datos de la API
// Hcae rmencion que esto se ejecuta una vez que el dom está "montado"
// onMounted(async() => {
//     // loading.value = true;
//     try {
//       const res = await fetch('https://jsonplaceholder.typicode.com/posts');
//       posts.value = await res.json()
//     } catch (error) {
//       console.log(error);
//     }finally {
//       setTimeout(() => {
//        loading.value = false
//      }, 2000);
//     }
// });


// Con esta opción estamos desarrollando el fetch directamente y esto es posible gracias al setup
// fetch('https://jsonplaceholder.typicode.com/posts')
//   .then((res) => res.json())
//   .then((data) => {posts.value = data
//   })
//   .finally(() => {
//     setTimeout(() => {
//       loading.value = false
//     }, 2000);
//   })


// Esta es la opción correcta para el consumo de los datos de auna API por debido que ase implementa 
// promesa y estamos obteneiendo los datos con la función asincrona
const fechtData = async () => {
  try {
      const res = await fetch('https://jsonplaceholder.typicode.com/posts');
      posts.value = await res.json()
    } catch (error) {
      console.log(error);
    }finally {
      setTimeout(() => {
       loading.value = false
     }, 2000);
    }
}

fechtData ();

const maxlength = computed(()=> posts.value.length);
</script>

<template>
  <LoadingSpinner v-if="loading"/>
  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mi post favorito: {{ favorito }}</h2>



    <PaginatePost 
        @next="next" 
        @prev="prev" 
        :inicio="inicio" 
        :fin="fin" 
        :maxlength = "maxlength"
        class="mb-2" 
    />

    <BlogPost 
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title" 
      :id="post.id" 
      :body="post.body" 
      :cambiarFavorito="cambiarFavorito"
      class="mb-2"
    ></BlogPost> 
   
  </div>
</template>


