<!-- AuthorDetails.vue -->
<template>
  <div class="row">
    <div class="eleven column" style="margin-top: 5%">
      <h2>{{title}}</h2>
    </div>
  </div>
</template>

<script>
  import {
    useRoute
  } from 'vue-router'

  export default {
    name: "Author Details",
    props: ['show', 'edit', 'create'],
    data() {
      return {
        title: "Author Data",
      }
    },
    mounted() {
    },
    methods: {
    }
  };
</script>