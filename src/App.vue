<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase Sample Application</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
            <label for="bookTitle">Author:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.author">
            <label for="bookTitle">URL:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.url">
            <input type="submit" class="btn btn-primary" value="Add Book">
          </div>
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books Lists</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Title
              </th>
              <th>
                Author
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td>
                <a v-bind:href="book.url">{{book.title}}</a>
              </td>
              <td>
                {{book.author}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import toastr from 'toastr'
// Initialize Firebase
import Firebase from 'firebase'

let config = {
  apiKey: "AIzaSyAPz57fFWrXqZUGdxF3XF-mY0AKVmzjZFc",
  authDomain: "vue-firebase-1709.firebaseapp.com",
  databaseURL: "https://vue-firebase-1709.firebaseio.com",
  projectId: "vue-firebase-1709",
  storageBucket: "vue-firebase-1709.appspot.com",
  messagingSenderId: "1071593172126"
};

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function(){
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function(book){
      booksRef.child(book['.key']).remove();
      toastr.success("success delete");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
