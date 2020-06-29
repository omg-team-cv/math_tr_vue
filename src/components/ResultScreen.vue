<template>
  <div class="alert alert-primary">
    <h3>Success: {{stats.success}} </h3>
    <h3>Error: {{stats.error}} </h3>
    <hr>
    <button 
    @click="$emit('repeat')"
    class="btn btn-primary">Repeat</button>
    <button 
    @click="$emit('nextLevel')"
    class="btn btn-primary">Next Level</button>
  </div>
</template>

<script>
  export default {
    props: ['stats']
  }
</script>

<style scoped>
h2, .btn{
  margin: 20px 0;
}
.btn{
  margin-right: 20px;
}

</style>