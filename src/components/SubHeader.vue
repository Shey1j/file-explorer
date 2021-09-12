<template>
  <div class="sub-header">
      <div v-if="!header">
    <h5>My Files</h5>
      </div>
      <div v-else>
          {{ header }}
      </div>
  </div>
</template>

<script>
export default {
    props: ['header'],
}
</script>

<style scoped>
.sub-header {
  padding: 1rem;
  display: flex;
  border-bottom: 1px solid #8080804a;
}

h5 {
    font-family: "Karla", sans-serif;
    font-weight: 400;
    font-size: 1.25rem;
}
</style>