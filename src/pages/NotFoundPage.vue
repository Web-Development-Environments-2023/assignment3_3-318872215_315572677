<template>
  <div class="container" :style="{ 'background-image': 'url(' + imageURL + ')' }">
    <h1>Four Oh Four you didn't</h1>
    <router-link to="/" exact>ET Go Home</router-link>

    <br><br><br><br><br><br><br><br><br><br><br><br><br><br>
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br>
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      imageURL: '',
      imageUrls: [
        require('@/assets/FNF.jpg'),
        require('@/assets/FNF1.jpg'),
      ],
    };
  },
  created() {
    const randomIndex = Math.floor(Math.random() * this.imageUrls.length);
    this.imageURL = this.imageUrls[randomIndex];
  },
};
</script>

<style>
.container {
  background-repeat: no-repeat;
  background-size: cover;
}
</style>