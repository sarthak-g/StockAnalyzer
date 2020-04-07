<template>
  <div class="hello">
    <div class="container">
      <form class="navbar-form m-2">
        <div class="input-group no-border">
          <input type="text" value="" class="form-control" placeholder="Search..."
          v-model="stock">
          <button @click="update(stock)" class="btn btn-white btn-round bn-just-icon ml-3">
            <i class="material-icons">search</i>
          </button>
        </div> 
      </form>

      <div class="row">
        <div class="col-md-3">
          <div class="card card-stats card-background_first">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">content_copy</i>
              </div>
              <p class="card-category">Beta</p>
              <h4 class="card-title">TBD</h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="card card-stats card-background_second">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">store</i>
              </div>
              <p class="card-category">CEO</p>
              <h4 class="card-title">TBD</h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="card card-stats card-background_third">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">info</i>
              </div>
              <p class="card-category">Company name</p>
              <h4 class="card-title">TBD</h4>
            </div>
          </div>
        </div>
        <div class="col-md-3">
          <div class="card card-stats card-background_fourth">
            <div class="card-header card-header-icon">
              <div class="card-icon">
                <i class="material-icons">storage</i>
              </div>
              <p class="card-category">Sector</p>
              <h4 class="card-title">TBD</h4>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'StocksInfo',
  props: {
    msg: String
  },
  data(){
    return {
      stock: ''
    }
  },
  methods: {
    update(stock){
      console.log(stock);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.card-background_first{
  background-color: lightseagreen;
  color: white;
}
.card-background_second{
  background-color: darkgreen;
  color: white;
}
.card-background_third{
  background-color: gold;
  color: white;
}
.card-background_fourth{
  background-color: rebeccapurple;
  color: white;
}
</style>
