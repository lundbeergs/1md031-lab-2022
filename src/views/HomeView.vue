<template>
  <!-- <div>
    <div>
      Burgers
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div> -->
  <body>
  <header> 
            <h1> Välkommen till Ångströms hamburgare</h1>
            <img class="header_image" src="https://www.adobe.com/content/dam/cc/us/en/creativecloud/video/discover/types-of-shots-in-films/desktop/types-of-shots-in-film_P1_900x420.jpg.img.jpg"/>
        </header>
        <main>
            <section id="burgers"> 
                <h2> Välj hamburgare</h2>
                <p> Det är här du väljer din hamburgare</p>
                <div class="wrapper1">
                  <Burger v-for="burger in burgers" v-bind:burger="burger" v-bind:key="burger.name" v-on:orderedBurgers="addToOrder($event)"/>

                <!-- <div class="box a">
                    <h3> The Cheesy Burger</h3>
                    <img src="https://max-images.futureordering.com/images/product/7042/87FAD18CC8D6AF9262E5CB4EA0121FD6D/652x606.PNG" alt="En bild på The Cheesy Burger, med saftigt bröd, ost, tomat, rödlök och paty" title="The Cheesy Burger">
                    <ul>
                        <li>Går att göra vegetarisk</li>
                        <li>Innehåller <span class = "allergy" > gluten </span> </li>
                        <li>Innehåller <span class = "allergy" > mjölk </span></li>
                    </ul> 
                </div>
                <div class="box b">
                    <h3> The Halloumi Burger</h3>
                    <img src="https://max-images.futureordering.com/images/product/12355/03677BCD4ACBAA32B2B9AC3003CD0BFCD/652x606.PNG" alt="En bild på The Halloumi Burger, med saftigt bröd, friterad halloumi, vegansk bacon, rödlök och BBQ sås" title="The Halloumi Burger" >
                    <ul>
                        <li>Vegetarisk </li>
                        <li>Innehåller <span class = "allergy" > gluten </span> </li>
                        <li>Innehåller <span class = "allergy" > mjölk </span></li>
                    </ul> 
                </div>
                <div class="box c">
                    <h3> The BBQ Chicken Burger</h3>
                    <img src="https://max-images.futureordering.com/images/product/12518/6A61F2B75F7816BCBE075ED18A3CFA42D/652x606.PNG" alt="En bild på The BBQ Chicken Burger, med saftigt bröd, coleslaw, BBQ sås och friterad kyckling" title="The BBQ Chicken Burger" >
                    <ul>
                        <li>Går att göra vegetarisk</li>
                        <li>Innehåller <span class = "allergy" > gluten </span></li>
                        <li>Innehåller <span class = "allergy" > mjölk </span></li>
                    </ul> 
                </div> -->
                </div>
            </section>
            <section id="info">
                <h2> Customer information</h2>
                <p> Det är här du ger oss nödvändig information</p>
                <h3> Leveransinformation</h3>
                <form>
                <p>
                    <label for="fullname">Full name</label><br>
                    <input type="text" id="fullname" v-model="fullname" required="required" placeholder="First and last name">
                </p>
                <p>
                    <label for="email"> E-mail</label><br>
                    <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
                </p>
                <!-- <p>
                    <label for="streetname"> Street</label><br>
                    <input type="text" id="streetname" v-model="streetname" placeholder="Streetname">
                </p> -->
                
                <!-- <p>
                    <label for="streetnumber"> Streetnumber</label><br>
                    <input type="number" id="streetnumber" v-model="streetnumber" placeholder="Streetnumber">
                </p> -->
                <h3>Betalningsalternativ</h3>
                <p>
                    <label for="payment"></label>
                    <select id="payment" v-model="payment">
                        <option>Swish</option>
                        <option selected="selected">Kort</option>
                        <option>Kontant</option>
                        <option>Klarna</option>
                    </select>
                 </p>
                 <p> Något annat vi behöver veta om? </p>
                 <textarea cols="30" rows="4" maxlength="300"> </textarea>
                 <h3>Vänligen fyll i nedan</h3>
                 <p>
                    <input type="radio" id="Male" v-model="gender" value="Male" checked>
                    <label for="Male"> Male</label>
                </p>
                <p>
                    <input type="radio" id="Female" v-model="gender" value="Female">
                    <label for="Female"> Female</label>
                </p>
                <p>
                    <input type="radio" id="Other" v-model="gender" value="Other">
                    <label for="Other"> Other </label>
                </p>
                </form>

                <div class="wrapper2">
                <div id="map" v-on:click="setLocation">
                  <div id="dots">
                    <div v-bind:style="{
                      left: location.x + 'px',
                      top: location.y + 'px'
                    }">
                      T
                    </div>
                  </div>
              </div>
            </div>
                
            </section>
            <button id="orders" v-on:click="markDone()">
                    Place order
                  </button>
        </main>
        <hr>
        <footer>
            <p> &COPY; 2022 Ångströms Hamburgare AB </p>
        </footer>
      </body>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

//Object Constructor Function

// function MenuItem(n, kC, pic, lac, glu) {
//     this.name = n; // The *this* keyword refers to the object itself
//     this.kCal = kC;
//     this.url = pic;
//     this.lactose = lac;
//     this.gluten = glu;
// }

// Objects are then instantiated using the *new* keyword
// const item = [new MenuItem('The Cheesy Burger', '300', 'https://max-images.futureordering.com/images/product/7042/87FAD18CC8D6AF9262E5CB4EA0121FD6D/652x606.PNG', 'true', 'true'), 
//               new MenuItem('The Halloumi Burger', '400', 'https://max-images.futureordering.com/images/product/12355/03677BCD4ACBAA32B2B9AC3003CD0BFCD/652x606.PNG', 'false', 'true'), 
//               new MenuItem('The BBQ Chicken Burger', '500', 'https://max-images.futureordering.com/images/product/12518/6A61F2B75F7816BCBE075ED18A3CFA42D/652x606.PNG', 'false', 'false')];

// console.log( item ); 

export default {
  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {
              burgers: menu,
              // [ {name: "small burger", kCal: 250},
              //    {name: "standard burger", kCal: 450},
              //    {name: "large burger", kCal: 850}
              //  ]
              fullname: '',
              email: '',
              // streetname: '',
              // streetnumber: '',
              payment: 'Kort',
              gender:'',
              orderedBurgers: {},
              location: { x: 0,
                          y:0
                        }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

   
    addToOrder: function (event) {
      console.log("I addToOrder")
      this.orderedBurgers[event.name] = event.amount;
      console.log("event:", event);
      console.log("event.name:", event.name);
      console.log("event.amount: " + event.amount);
    },

    setLocation: function (event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                      y: event.currentTarget.getBoundingClientRect().top};
        this.location = {
                      x: event.clientX - 10 - offset.x,
                      y: event.clientY - 10 - offset.y
        }
     },

    markDone:function() {
      console.log("I markDone")
      console.log(this.fullname, this.email, this.payment, this.gender)
      console.log(this.orderedBurgers)
      
      socket.emit("addOrder", { 
          orderId: this.getOrderNumber(), 
          costName: this.fullname,
                    // Gatunamn: this.streetname,
                    // Gatunummer: this.streetnumber,
          costEmail: this.email,
          costPay: this.payment,
          costGender: this.gender,
          orderItems: this.orderedBurgers,
          details: {x: this.location.x,
                  y: this.location.y

          } 
        });
    },
  }
}
</script>

<style>
  /* @import url("reset.css"); */
@import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';


body {
    font-family: Arial;
    font-size: 100%;
}

header{
    height: 20vw;
    margin: 2vw;
    overflow: hidden;
    font-size: 2vw;
}

header > h1 {
    position: absolute;
    padding-left: 10%;
    padding-top: 5%;
    
}

.header_image{
    object-position: 50%;
    /* width: 100vw; */
    width: 94vw;
    object-fit: cover;
    margin-right: 2vw;
    /* padding-right: 2%; */
    opacity: 0.5;
    
}

section {
    margin: 2vw;
    padding: 2vh; 
}

.wrapper1 {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 30% 30% 30%;
    color: white;
}

.box {
    background-color: black;
    border-radius: 5px;
    padding: 10px;
}

#burgers img{
    width: 90%;
    height: auto;
    overflow: hidden;
    object-position: center;
}
 

footer{
    margin: 2vw;
}
/* 
div{
    margin: 1vw;
    padding: 2vw;
} */

div > h3 {
    text-align: center;
}

.allergy {
    font-weight: bold;
}

#burgers {
    background-color: black;
    color: white;
    border: 2px dotted white;
}

#info {
    border: 2px dotted black;
}

button{
    background-color: white;
    margin: 2vw;
    margin-top: 0vh;
    border: 2px solid black;
    padding: 15px 32px;
    border-radius: 10px;
    text-align: center;
    /* text-decoration: none; */
    display: inline-block;
}

button:hover {
    background-color: green;
    cursor: pointer;
}

.wrapper2 {
  width: 100%;
  height: 50vh;
  overflow: scroll;
  /* background-repeat: no-repeat; */
  /* background-size: cover; */
}

#map {
  width: 1920px;
  height: 1078px;
  background: url("/public/img/polacks.jpg");
}


/* Kommentar */
</style>