<template>
  <div>
    <!-- header and navigation -->
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <a href="/">Yet another HN clone</a>

        <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu">
          <div class="navbar-start"></div>

          <div class="navbar-end">
            <router-link class="navbar-item" to="/">
              Top
            </router-link>
            <router-link class="navbar-item" to="/new">
              New
            </router-link>
            <router-link class="navbar-item" to="/best">
              Best
            </router-link>
          </div>
        </div>

    </nav>

    <!-- the page view etc -->
    <div id="app">
      <router-view/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
@import 'assets/app.scss';
#app {
  max-width: 800px;
  margin: 0 auto;
  margin-top: 15px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.navbar {
  max-width: 1000px;
  margin: 0 auto;
}
h1 {color:red}
</style>
