<template>
  <div class="text-center mt-4">
    <h1>{{ msg }}</h1>
    
  </div>
</template>

<script>
export default {
  name: 'ToDo',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
 body{
  background-color: orange;
 }
</style>
