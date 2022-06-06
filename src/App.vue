<template>
  <div id="app">
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav> -->
    <router-view/>
  </div>
</template>

<style>

* {
  font-family: 'Inter', sans-serif;
  box-sizing: border-box;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  width: 60%;
  margin: 0 auto;
  border: 1px solid #ddd;
  border-radius: 5px;
  /* color: #2c3e50; */
}

body {
  margin: 0;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

.text-muted {
  color: rgb(116, 116, 116) !important;
}

.text-blue {
    color: #0052f5;
}

.text-green {
    color: #00a88a;
}

.text-red {
  color: #eb5757;
}

.small {
  font-size: 0.7rem;
}

.font-weight-500 {
  font-weight: 500 !important;
}

.font-weight-600 {
  font-weight: 600 !important;
}

.bigger {
  font-size: 1.05rem !important;
}

.hover-pointer {
  cursor: pointer;
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

h6 {
  margin-bottom: 5px;
}

p, button {
  font-size: 0.775rem;
}

h4 {
  margin: 0;
}

p {
  margin-top: 0;
}

.mb-0 {
  margin-bottom: 0 !important;
}

.mb-2 {
  margin-bottom: 10px;
}

.pt-0 {
  padding-top: 0 !important;
}

.font-500 {
  font-weight: 500;
}

@media only screen and (max-width: 820px) {
  #app {
    width: 80%;
  }    
}

@media only screen and (max-width: 480px) {
  #app {
    width: 100%;
  }    
}

</style>
