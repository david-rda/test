<template>
  <div>
    <h1>hello</h1>
    <h1>hello</h1>
    <h1>hello</h1>
  </div>
</template>
<script>

import { ref, onBeforeMount, computed } from 'vue';
import axios from 'axios';
import { useHead } from '@vueuse/head';

export default {
  setup() {
    // Define a reactive property for name
    const title = ref("df");
    const description = ref("df");

    // Fetch data and update name on component mount
    onBeforeMount(() => {
      axios.get("https://jsonplaceholder.typicode.com/posts/1")
        .then((res) => {
          title.value = res.data.title;
          description.value = res.data.body;
        })
        .catch((error) => {
          console.error('Error fetching data:', error);
        });
    });

      const authorMeta = computed(() => ({
        property: "og:title",
        content: title.value // Use computed to reactively update meta content
      }));

      const authorMeta1 = computed(() => ({
        property: "og:description",
        content: description.value // Use computed to reactively update meta content
      }));

      // Set up metadata using useHead
      useHead({
        title: 'My awesome site',
        meta: [
          authorMeta.value, // Use computed property here
          authorMeta1.value // Use computed property here
        ]
      });

    return {
      title, // Expose name to the template if needed
      description // Expose name to the template if needed
    };
  },
};

</script>
