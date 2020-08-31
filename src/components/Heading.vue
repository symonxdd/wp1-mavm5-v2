<template>
  <div id="container"> <!-- this div is vital, code breaks otherwise -->
    <img id="logo" src="@/assets/caduceus.svg"/>
    <p id="caption">Praktijk Adélie Ménard</p>
  </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
    font-family: 'Montserrat', sans-serif;
}

#logo {
    float: left;
    width: 130px;
    height: auto;
    padding-left: 20px;
}

#caption {
  font-size: 25px;
  color: #282C34;
  float: left;
  padding-top: 50px;
}
</style>
