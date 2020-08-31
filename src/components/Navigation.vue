<template>
    <nav id="nav">
      <ul>
        <li><router-link to="/">Home</router-link></li>
        <li><router-link to="/afspraken">Afspraken</router-link></li>
      </ul>
    </nav>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
    font-family: 'Montserrat', sans-serif;
}

nav {
    margin: 30px 0 20px 0;
    float: right;
}
ul {
    list-style-type: none;
    margin: 0;
    overflow: hidden;
    background-color: white;
    color: #282C34;
}

li {
    float: left;
}

li a {
    display: block;
    color: #282C34;
    padding: 14px 16px;
    text-decoration: none;
    transition: 0.1s;
}

li a:hover {
    background-color: #e9e9e9;
}
</style>
