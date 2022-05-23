<template>
  <main >
  
  <router-view/>
  </main>
  
</template>

<style>
body{
overflow-x: hidden;
}
.pointer{
  cursor: pointer;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
a:not(.link), a:not(.link):hover{
text-decoration: none;
color: black;
}
main{
  padding-top:20px ;
margin: 0px auto;
max-width: 1024px;
text-align: start;
}
nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
