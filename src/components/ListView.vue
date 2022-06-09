<template>
        <ul v-if="!loading && data && data.length">
    <li v-for="character of data" :key="character">
      <p ><strong>{{character.firstname}}</strong></p>
      <p> {{ character.lastname }} </p>
    </li>
  </ul>

    <p v-if="loading">
   Still loading..
  </p>
  <p v-if="error">

so can't find what you're looking for

  </p>
</template>

<script>
import { onMounted } from '@vue/runtime-core';

export default {
    props: [ 'office' ],
    setup() {
    const data = ref(null);
    const loading = ref(true);
    const error = ref(null);

    function fetchData() {
        loading.value = true;
        // Fetching
          return fetch('https://officeapi.dev/api/characters/', {
            method: 'GET',
            headers: {
              'content-type': 'application/json'
            }
          })
          .then(res => {
            // a non-200 response code
            if (!res.ok) {
              // create error instance with HTTP status text
              const error = new Error(res.statusText);
              error.json = res.json();
              throw error;
            }

          return res.json();
      })
      .then(json => {
        // set the response data
        data.value = json.data;
      })
      .catch(err => {
        error.value = err;
        // In case a custom JSON error response was provided
        if (err.json) {
          return err.json.then(json => {
            // set the JSON response message
            error.value.message = json.message;
          });
        }
      })
      .then(() => {
        loading.value = false;
      });
    }

    onMounted(() => {
      fetchData();
    });

    return { data, loading, error };
  }
}
</script>

<style>

</style>