<template>
  <div class="home">
     <h1>This is main page</h1>
    
  </div>
</template>

<script>


export default {
  name: 'home',
  data: function () {
    return {
      info: 'not updated'
    }
  },
  mounted() {

    axios
      .get('https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json?api-key=FXB1aLiY1JuV0nbAhGEJD3LVKDACApxc')
      .then(response => (this.info = response));
  }
}
</script>
