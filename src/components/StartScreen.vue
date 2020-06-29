<template>
  <div class="alert alert-secondary">
    <h3>Are you ready?</h3>
    <button @click="$emit('onStart')"
    class="btn btn-success">Start</button>
  </div>
</template>

<script>
  export default {
    
    
  }
</script>

<style scoped>
.alert{
  text-align: center;
}
h3, .btn{
  margin: 30px 0;
}
</style>