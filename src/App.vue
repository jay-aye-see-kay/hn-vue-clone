<template>

  <div class="container container-narrow">
    <!-- header and navigation -->
    <div class="header bg-orange">
      <nav class="navbar navbar-expand-lg navbar-light">
        <h1 class="site-title">
          <a class="nav-link" to="/">Hacker News</a>
        </h1>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav mr-auto">
            
            <li class="nav-item">
              <router-link class="nav-link" to="/">Top</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/new">New</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/best">Best</router-link>
            </li>
          </ul>
        </div>
      </nav>
    </div>

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
.container-narrow {
  max-width: 1000px;
  margin: 0 auto;
}
.bg-orange {
  background-color: #ff6600;
}
a.nav-link {
    padding: 5px 10px;
}
a.router-link-exact-active {
    border-bottom: 2px solid black;
}
.site-title {
  font-size: 1.5rem;
}
.card {
  margin-bottom: 5px;
}
</style>
