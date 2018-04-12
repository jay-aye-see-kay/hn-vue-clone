<template>
  <div>
    <!-- header and navigation -->
    <nav class="navbar" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <h1>Yet another HN clone</h1>

        <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu">
          <div class="navbar-start"></div>

          <div class="navbar-end">
            <a class="navbar-item">
              Top
            </a>
            <a class="navbar-item">
              New
            </a>
            <a class="navbar-item">
              Best
            </a>
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
</style>
