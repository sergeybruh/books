<template>
<div class="container">
  <div class="topbooks">
    <h1>This is an topbooks page</h1>
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text">filter by title</span>
      </div>
      <input @change="getData" v-model="name" type="text" class="form-control" aria-label="Amount (to the nearest dollar)">
    </div>
    <div class="input-group mb-3">
    <div class="input-group-prepend">
      <label class="input-group-text" for="inputGroupSelect01">Sort by Publisher</label>
    </div>
    
    <select @change="selectChange" class="custom-select" id="inputGroupSelect01">
      <option></option>   
      <option v-for="item in authors.data.results" :value="item.publisher"> {{item.publisher}}</option>   
    </select>
  </div>
<table class="table">
  <thead>
    <tr>
      <th scope="col">ranks_history -> rank</th>
      <th scope="col">Title</th>
      <th scope="col">Author</th>
      <th scope="col">Publisher</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="item in info.data.results">
      <th scope="row" >{{item.ranks_history[0].rank}}</th>
      <td>{{item.title}}</td>
      <td>{{item.author}}</td>
      <td>{{item.publisher}}</td>
    </tr>
  </tbody>
</table>
  </div>
</div>
</template>
<script>

export default {
  name: 'Topbooks',
  data: function () {
    return {
      info: {
        data: {
          results: [{
            ranks_history: {
              rank: ""
            }
          }]
        }  
      },
      name: "",
      publisher: "",
      authors: {
        data: {
          results: [{
            ranks_history: {
              rank: ""
            }
          }]
        }  
      }
    }
  },
  methods: {
    getData: function (event) {
      axios
        .get(`https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json?api-key=FXB1aLiY1JuV0nbAhGEJD3LVKDACApxc&publisher=${this.publisher}&title=${this.name}`)
        .then(response => (this.info = response))
        .then(response => (this.info.data.results.forEach(
          function(item, index) {
            if(typeof item.ranks_history[0] === "undefined") {
              item.ranks_history[0] = {
                rank: 0
              }
            }
          }

        )))
    },
    getAuthors: function (event) {
      axios
        .get(`https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json?api-key=FXB1aLiY1JuV0nbAhGEJD3LVKDACApxc`)
        .then(response => (this.authors = response));
    },
    selectChange: function(event){
      this.publisher = event.target.value;
      this.getData();
    }
  },
  mounted() {
    this.getData();
    this.getAuthors();
  }
}
</script>