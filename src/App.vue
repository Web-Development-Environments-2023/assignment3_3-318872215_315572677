<template>
  <div id="app">
    <header>
      <div class="header-container">
        <div class="header-left">
          <!-- left bar nav -->
          <router-link :to="{ name: 'main' }"> Vue Recipes</router-link>|
          <router-link :to="{ name: 'search' }"> Search </router-link>|
          <router-link :to="{ name: 'about' }"> About</router-link>|

          <!-- when user log in create recipe & presonal show -->
          <button v-if="$root.store.username" class="created-recipe" @click="showRecipeModal">Create Recipe</button>
          <div v-if="!!$root.store.username" class="dropdown">
            <button class="dropdown-button">Personal</button>
            <div class="dropdown-content">
              <tr><router-link :to="{ name: 'myFavorite' }"> Favorites</router-link></tr>
              <tr><router-link :to="{ name: 'myRecipe' }"> My Recipes</router-link></tr>
              <tr><router-link :to="{ name: 'myFamily' }"> Family Recipes</router-link></tr>
            </div>
          </div>
        </div>

        <!-- right bar nav -->
        <div v-if="!$root.store.username" class="guest">
          <span>Guest:</span>
          <router-link :to="{ name: 'register' }">Register</router-link>
          <router-link :to="{ name: 'login' }">Login</router-link>
        </div>

        <div v-if="!!$root.store.username" class="dropdown">
          <button class="dropdown-button"><span>{{ $root.store.username }}</span></button>
          <div class="dropdown-content">
            <tr><router-link :to="{ name: 'account' }">Account</router-link></tr>
            <tr><button class="logout-btn" @click="Logout">Logout</button></tr>
          </div>
        </div>



        <!-- <div v-else class="user-login">
          <button class="dropdown-button"><span>{{ $root.store.username }}</span></button>
          <div class="dropdown-content">
            <tr><router-link :to="{ name: 'account' }">Account</router-link></tr>
            <tr><button @click="Logout">Logout</button></tr>
          </div>
          <button @click="Logout">Logout</button>
        </div> -->
      </div>
    </header>

    <!-- Main page -->
    <main>
      <div class="main-section">
        <h1 class="main-title">Grandma's recipes and others</h1>
        <p class="main-description">Unlock the Secrets of Flavor</p>
      </div>
      <router-view />
    </main>

    <!-- footer -->
    <footer>
      <div class="footer-container">
        <p>&copy; 2023 All rights reserved to Oran-Shay.</p>
      </div>
    </footer>

  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      isLogged: !!this.$root.store.username,
      modalMessage: "",
      showCreateRecipe: false
    };
  },
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
    showModal(message) {
          this.modalMessage = message;
          this.$refs["my-modal"].show();
      },
    hideModal() {
          this.modalMessage = "";
          this.$refs["my-modal"].hide();
      },
    toggleModal() {
          // We pass the ID of the button that we want to return focus to
          // when the modal has hidden
          this.$refs["my-modal"].toggle("#toggle-btn");
      },
    showRecipeModal(){
          this.$refs["recipe-modal"].show();
      },
    hideRecipeModal(){
          this.$refs["recipe-modal"].hide();
      }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

// #nav {
//   padding: 30px;
// }

// #nav a {
//   font-weight: bold;
//   color: #2c3e50;
// }

// #nav a.router-link-exact-active {
//   color: #42b983;
// }


body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}
header {
  background-color: #333;
  padding: 10px 20px;
  border-bottom: 2px solid #f2f2f2;
}
.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.header-left {
  display: flex;
  align-items: center;
}
.logo {
  font-size: 24px;
  font-weight: bold;
  text-decoration: none;
  color: #fff;
  margin-right: 20px;
}
.search {
  background-color: #ff4081;
  border: none;
  padding: 8px 15px;
  cursor: pointer;
  color: white;
  font-weight: bold;
  border-radius: 20px;
  transition: background-color 0.3s ease;
}
.search:hover {
  background-color: #19e7c1;
  border-radius: 20%;
  color: #ffffff;
  text-decoration: none;
}
.about {
  background-color: #ff4081;
  border: none;
  padding: 8px 15px;
  cursor: pointer;
  color: white;
  font-weight: bold;
  border-radius: 20px;
  transition: background-color 0.3s ease;
}
.about:hover {
  background-color: #ffffff;
  border-radius: 20%;
  color: #333;
  text-decoration: none;
}
header a {
  color: #ffffff;
  text-decoration: none;
  margin-right: 10px;
  font-weight: bold;
}
.header-middle {
  display: flex;
  align-items: center;
}

//~~~~~~~~~~~~~~~~~~~~ create ~~~~~~~~~~~~~~~~~~~~
.created-recipe {
  background: linear-gradient(45deg, #ff4081, #d5004e);
  border-radius: 20px;
  padding: 8px 15px;
  cursor: pointer;
  color: white;
  font-weight: bold;
  margin-right: 10px;
}

.created-recipe:hover {
  background: linear-gradient(45deg, #d5004e, #ff4081);
}

//~~~~~~~~~~~~~~~~~~~~ DROPDOWN ~~~~~~~~~~~~~~~~~~~~
.dropdown {
  position: relative;
  margin-right: 10px;
}
.dropdown-button {
  background-color: #ff4081;
  border: none;
  padding: 8px 15px;
  cursor: pointer;
  color: rgb(255, 255, 255);
  font-weight: bold;
  border-radius: 20px;
  transition: background-color 0.3s ease;
}
.dropdown-button:hover {
  background-color: #d5004e;
}
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #bfa7c4;
  min-width: 200px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  z-index: 1;
  border-radius: 4px;
  padding: 10px;
}
.dropdown:hover .dropdown-content {
  display: block;
}

//~~~~~~~~~~~~~~~~~~~~ GUEST|USER ~~~~~~~~~~~~~~~~~~~~
.guest span {
  margin-right: 10px;
  font-weight: bold;
  color: white;
}
.user-login span {
  margin-right: 10px;
  font-weight: bold;
  color: #ffffff;
  background-color: #f3e5f5;
  border: 2px solid #ff4081;
  border-radius: 20px;
  padding: 8px 15px;
  transition: background-color 0.3s ease;
}
.user-login span:hover {
  background-color: #ff4081;
  color: white;
}

//~~~~~~~~~~~~~~~~~~~~ MAIN ~~~~~~~~~~~~~~~~~~~~
main {
  padding: 10px;
}
.main-section {
  background-image: linear-gradient(to bottom right, #ff8c00, #ff0080);
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: white;
}
.main-title {
  font-size: 4rem;
  margin-bottom: 1rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}
.main-description {
  font-size: 1.8rem;
  margin-bottom: 2rem;
  font-style: italic;
}
.logout-btn {
  background: transparent;
  border: none;
  padding: 0;
  font: inherit;
  color: transparent;
  cursor: pointer;
  color: #333;
  font-weight: bold;
  margin-right: 10px;
}
.logout-btn:hover {
  color: #0056b3;
  text-decoration: underline;
}

//~~~~~~~~~~~~~~~~~~~~ FOOTER ~~~~~~~~~~~~~~~~~~~~
footer {
  background-color: #333;
  color: white;
}
.footer-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 460px;
  margin: 0 auto;
}
.footer-left p {
  font-size: 18px;
}

</style>
