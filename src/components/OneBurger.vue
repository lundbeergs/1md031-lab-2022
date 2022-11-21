<template>
  
    <div class="box">
        <h3> {{ burger.name}}</h3>
        <img class="burger_image" v-bind:src="burger.url" alt="">
        <ul>
            <li> {{burger.kCal}} kCal </li>
            <li v-if="burger.lactose"> Innehåller <span class = "allergy" > laktos </span> </li>
            <li v-if="burger.gluten"> Innehåller <span class = "allergy" > gluten </span></li>
        </ul> 
    </div>

  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  

div > h3 {
    text-align: center;
}

.allergy {
    font-weight: bold;
}

.burger_image{
    width: 250px;
    height: auto;
}

  </style>
  