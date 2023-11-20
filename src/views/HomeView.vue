<template>
<!--    <header class="headline">
        <h1 style="position:absolute; left: 250px; margin: 200px;">Welcome to BurgerHeaven</h1>
        <img src="img/background.jpg" style="opacity:0.5;  position:relative;">
    </header>   -->
    <main>
        <section class="space burgers">
            <h1 style="margin: 10px;">Select Burger</h1>
     <div class="wrapper">
     <Burger v-for="burger in burgers"
              v-bind:burger="burger"
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
     </div>

        </section>
        <section class="space customerinfo">
          <div class="space">
            <h2>Customer information</h2>

                <h3>Delivery information</h3>
                <p>
                    <label for="firstname">Full name</label><br>
                    <input type="text" id="firstname" v-model="fn" required="required" placeholder="First- and Last name">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                </p>
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
            </div>
            <div class="space">
                <input type="radio" id="female" v-model="drone" value="female" />
                <label for="female">Female</label>
            </div>
            <div class="space">
                <input type="radio" id="male" v-model="drone" value="male" />
                <label for="male">Male</label>
            </div>
            <div class="space">
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
        Indicate point of delivery by clicking on the map
    </footer>
  <div id="wrappermap">
    <div id="map" v-on:click="setLocation">
              <div id="dot" v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px'}" v-bind:key="'dots' + key">
                T
              </div>
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import burgerList from '../assets/menu.json'


const socket = io();


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
                },
      orderNr: 0
    }
  },
  methods: {
    getOrderNumber: function () {
//      return Math.floor(Math.random()*100000);
      return this.orderNr +=1;
    },
    addOrder: function (event) {
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           fn: this.fn,
                                           em: this.em,
                                           rcp: this.rcp,
                                           drone: this.drone},
                                orderItems: [this.orderedBurgers]
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
      this.location = {x: event.clientX - 10 - event.currentTarget.getBoundingClientRect().left,
                    y: event.clientY - 10 - event.currentTarget.getBoundingClientRect().top};
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
    margin: 5px;
}

.burgerspace{
    padding: 60px;
}

.ubspace {
    padding: 10px;
}

.ubspace:hover {
   background-color: blue;
   cursor: grab;
}

.headline {
    text-align: center;
    overflow:hidden;
    height: 400px;
    background-image: url("../../public/img/background.jpg");
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
    position:relative;
        cursor: crosshair;
  }
  #wrappermap {
    overflow: scroll;
    width: 800px;
    height: 400px;
  }

  #dot {
        position: absolute;
        background: red;
        color: white;
        border-radius: 10px;
        width:20px;
        height:20px;
        text-align: center;

  }
</style>