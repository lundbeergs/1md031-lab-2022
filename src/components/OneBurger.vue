<template>
  
    <div class="box">
        <h3 v-bind:key="burger.name"> {{ burger.name}}</h3>
        <img class="burger_image" v-bind:src="burger.url" alt="Burger">
        <ul>
            <li> {{burger.kCal}} kCal </li>
            <li v-if="burger.lactose"> Innehåller <span class = "allergy" > laktos </span> </li>
            <li v-if="burger.gluten"> Innehåller <span class = "allergy" > gluten </span></li>
        </ul> 
        <div class="burgercount">
          <button v-on:click="remove"> - </button> {{amountOrdered}}
          <button v-on:click="add"> + </button>
        </div>
    </div>

  </template>
  
  <script>

  export default{

    name: 'OneBurger',
    props: {
      burger: Object
    },

    data: function () {
    return {
      amountOrdered: 0,
    }
    },


    methods: {
      add: function() {
        console.log("I add")
        this.amountOrdered++;
        this.$emit('orderedBurgers', 
              {name: this.burger.name,
              amount: this.amountOrdered
            }
            );
      },

      remove: function(){
        console.log("I remove")
        if(this.amountOrdered > 0 ){
          this.amountOrdered--;
        }

        this.$emit('orderedBurgers',
              {name: this.burger.name,
              amount: this.amountOrdered
              }
              );
      },
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
  