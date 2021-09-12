<template>
  <div class="sub-header">
      <div v-if="!header">
    <h5>My Files</h5>
      </div>
      <div v-else>
          <h5>My Files <p v-for="(item) in header" :key="item.id"><img src="../assets/right.png" alt="right arrow" /> {{ item.name }}</p></h5>
      </div>
  </div>
</template>

<script>
export default {
    props: {'header': Array},
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
    display: flex;
    column-gap: 1rem;
    align-items: center;
}

img {
    height: 1rem;
}
</style>