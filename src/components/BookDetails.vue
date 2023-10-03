<!-- BookDetails.vue -->
<template>
  <div class="row">
    <div class="eleven column" style="margin-top: 5%">
      <h2>{{title}}</h2>
      <form>
        <div class="row">
          <div class="six columns">
            <label for="titleInput">Title</label>
            <input class="u-full-width" type="text" v-model="book.title">
          </div>
          <div class="six columns">
            <label for="authorInput">Author</label>
            <input class="u-full-width" type="text" v-model="book.author">
          </div>
        </div>
        <div class="row">
          <div class="six columns">
            <label for="publisherInput">Publisher</label>
            <input class="u-full-width" type="text" v-model="book.publisher">
          </div>
          <div class="six columns">
            <label for="editionInput">Edition</label>
            <input class="u-full-width" type="text" v-model="book.edition">
          </div>
        </div>
        <div class="row">
          <div class="six columns">
            <label for="copyrightInput">Copyright</label>
            <input class="u-full-width" type="number" v-model="book.copyright">
          </div>
          <div class="six columns">
            <label for="languageInput">Language</label>
            <input class="u-full-width" type="text" v-model="book.language">
          </div>
        </div>
        <div class="row">
          <router-link class="button button-primary" to="/book">Back</router-link>
          <a v-if='edit' class="button button-primary" style="float: right" v-on:click="updateBook(book._id)">Update</a>
          <a v-if='create' class="button button-primary" style="float: right" v-on:click="createBook()">Create</a>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
  import {
    useRoute
  } from 'vue-router'

  import {
    v4 as uuidv4
  } from 'uuid';

  export default {
    name: "Book Details",
    props: ['create', 'edit', 'create'],
    data() {
      return {
        title: "Book Data",
        book: {}
      }
    },
    mounted() {
      const route = useRoute()
      if (route.params.id != null)
        this.GetBookById(route.params.id);
      else {
        this.book = {
          '_id': uuidv4(),
          'title': '',
          'edition': '',
          'copyright': 0,
          'language': '',
          'pages': 0,
          'author': '',
          'author_id': 0,
          'publisher': '',
          'publisher_id': 0
        };
      }
    },
    methods: {
      async GetBookById(id) {
        try {
          const httpResponse = await fetch(`${this.url}/bookGetById/${id}`, {
            headers: {
              'Accept': 'application/json'
            }
          });

          if (httpResponse.status === 200)
            this.book = await httpResponse.json();
          else {
            alert(await httpResponse.text());
            this.$router.push(`/book`);
          }
        } catch (error) {
          console.log(error);
          alert(`An error ocurred geting the book.`);
        }
      },
      async updateBook(id) {
        try {
          const httpResponse = await fetch(`${this.url}/bookUpdate/${id}`, {
            headers: {
              'Content-Type': 'application/json'
            },
            method: 'PUT',
            body: JSON.stringify(this.book)
          });
          if (httpResponse.status !== 200)
            alert(await httpResponse.text());
          else
            this.$router.push('/book');
        } catch (error) {
          console.log(error);
          alert(`An error ocurred updating the book.`);
        }
      },
      async createBook() {
        try {
          const httpResponse = await fetch(`${this.url}/bookNew`, {
            headers: {
              'Content-Type': 'application/json'
            },
            method: 'POST',
            body: JSON.stringify(this.book)
          });
          if (httpResponse.status !== 200)
            alert(await httpResponse.text());
          else
            this.$router.push('/book');
        } catch (error) {
          console.log(error);
          alert(`An error ocurred updating the book.`);
        }
      }
    }
  };
</script>