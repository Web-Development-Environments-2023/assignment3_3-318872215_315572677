<template>
    <div>
        <div id="background"></div>
        <h1>My Favorite</h1>
        <p>My Favorite Page</p>
    </div>
</template>

<script>
export default {

}
</script>

<style>
#background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url(../assets/food-background.jpg);
  background-size: cover;
  background-position: center;
  z-index: -1;
  opacity: 0.5;
}
</style>