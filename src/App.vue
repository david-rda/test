<template>
  <div>
    <h1>hello</h1>
    <h1>hello</h1>
    <h1>hello</h1>
  </div>
</template>
<script>
import { ref, onBeforeMount } from 'vue';
import axios from 'axios';
import { useHead } from '@vueuse/head';

export default {
  setup() {
    const title = ref("df");
    const description = ref("df");

    onBeforeMount(() => {
      axios.get("https://jsonplaceholder.typicode.com/posts/1")
        .then((res) => {
          title.value = res.data.title;
          description.value = res.data.body;

          // Update metadata here after fetching data
          updateMetadata();
        })
        .catch((error) => {
          console.error('Error fetching data:', error);
        });
    });

    const updateMetadata = () => {
      // Set up metadata using useHead
      useHead({
        title: title.value,
        meta: [
          { property: 'og:title', content: title.value },
          { property: 'og:description', content: description.value }
        ]
      });
    };

    return {
      title,
      description
    };
  },
};


</script>
