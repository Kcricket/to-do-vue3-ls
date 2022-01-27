<template>
<div class="bigwrapper">
  <div class="wrapper">
    <img alt="Vue logo" src="./assets/logo.png">
  </div>
  <AddBookForm/>
</div>

</template>

<script>
import AddBookForm from './components/AddBookForm.vue'

export default {
  name: 'App',
  components: {
    AddBookForm
  }
}
</script>

<style>
img{
  filter:contrast(200%);
}
.bigwrapper{
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  
  font-family: "Lucida Sans Unicode", "Lucida Grande", sans-serif;
  font-size: 18px;
  letter-spacing: 1.4px;
  word-spacing: 0.4px;
  color: #000000;
  font-weight: 400;


  margin-top: 60px;
}
.wrapper{
  display: flex;
  width: 70%;
  height: 300px;
  background-color: #F94892;
  filter:opacity(60%);
  background: url("../src/assets/metropolis.jpg") no-repeat scroll 50% 50%;

  justify-content: center;
}

</style>
