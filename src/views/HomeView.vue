<template>
    <header class="headline">
        <h1 style="position:absolute; left: 250px; margin: 200px;">Welcome to BurgerHeaven</h1>
        <img src="img/background.jpg" style="opacity:0.5;  position:relative;">
    </header>
    <main>
        <section class="space burgers">
            <h1 style="margin: 10px;">Select Burger</h1>
     <div>
     <Burger v-for="burger in burgers"
              v-bind:burger="burger"
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
     </div>

        </section>
        <section class="space customerinfo">
            <h2>Customer information</h2>
            <div>
                <h3>Delivery information</h3>
                <p>
                    <label for="firstname">Full name</label><br>
                    <input type="text" id="firstname" v-model="fn" required="required" placeholder="First- and Last name">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                </p>
            </div>
            <h3>Payment options</h3>
            <p>
                <label for="recipient">Recipient</label>
                <select id="recipient" v-model="rcp">
                    <option>Credit card</option>
                    <option>Swish</option>
                    <option>Klarna</option>
                    <option>Paypal</option>
                </select>
            </p>
            <h3>Gender</h3>
            <div>
                <input type="radio" id="female" v-model="drone" value="female" />
                <label for="female">Female</label>
            </div>
            <div>
                <input type="radio" id="male" v-model="drone" value="male" />
                <label for="male">Male</label>
            </div>
            <div>
                <input type="radio" id="do not wish to provide" v-model="drone" value="do not wish to provide" />
                <label for="do not wish to provide">Do not wish to provide</label>
            </div>
        </section>
            <p>
                <button class="ubspace" type="submit" v-on:click="addOrder">
                    <img src="img/Deliveryimage.png" style="width: 50px;">
                    Place order
                </button>
            </p>

    </main>
    <hr>
    <footer>
        End notes
    </footer>
  <div id="wrappermap">
    <div id="map" v-on:click="setLocation">
      click here
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import burgerList from '../assets/menu.json'


const socket = io();

/* function MenuItem(fn, url, kCal, glutenFree, lactoseFree, ingredients) {
    this.productName = fn;
    this.url = url;
    this.kCal = kCal;
    this.glutenFree = glutenFree;
    this.lactoseFree = lactoseFree;
    this.ingredients = ingredients
}*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerList,
      drone:"do not wish to provide",
      orderedBurgers: {},
      location: { x: 0,
                  y: 0
                }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      location.x=offset.x;
      location.y=offset.y;
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: [this.fn, this.em, this.rcp, this.drone, this.orderedBurgers]
                              }
                 );
    },
    testOrder: function (event) {
        console.log(this.em);
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function(event){

    }
  }
}
</script>

<style>
body {
   font-family: papyrus;
}

.allergen {
   font-weight: bold;
}

.burgers {
    border: 5px dotted #6a5acd;
    background-color: #b0c4de;
    color: white;
}

.customerinfo {
    border: 5px dotted #6a5acd;
}

.space {
    margin: 2px;
}

.burgerspace{
    padding: 60px;
}

.ubspace {
    padding: 10px;
}

button:hover {
   background-color: blue;
   cursor: grab;
}

.headline {
    text-align: center;
    overflow:hidden;
    height: 400px

}

img {
    width: 100%;
    height: auto;
}

.wrapper {
     display: grid;
     grid-gap: 10px;
     grid-template-columns: 33% 33% 33%;
}
  #map {
    width: 1920px;
    height: 1078px;
    background-image: url("../../public/img/polacks.jpg");
  }
  #wrappermap {
    overflow: scroll;
    width: 800px;
    height: 400px;
  }
</style>