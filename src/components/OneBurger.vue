<template>
            <div class="burger space" style="display: inline-block;">
                <h3>{{ burger.name }}</h3>
            <Burger v-for="burger in burgers"
              v-bind:burger="burger"
              v-bind:key="burger.name"
              />
              <img :src="burger.url">
                <ul>
                    <li>{{ burger.ingredients }}</li>
                    <li>lactose:{{ burger.lactoseFree }}, gluten:{{ burger.glutenFree }}</li>
                    <li>{{ burger.kCal }} kCal</li>
                </ul>


<div class="container">
  <div id="increment-count">
    <input type="image" id="up-arrow" src="img/up-arrow.jpg" v-on:click="inc"/>
  </div>
  <div id="total-count">
                {{amountOrdered}}
  </div>
  <div id="decrement-count">
    <input type="image" id="down-arrow" src="img/down-arrow.jpg" v-on:click="dec"/>
  </div>
</div>
            </div>

  </template>
  
  <script>
  export default {
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
        inc: function () {
          this.amountOrdered +=1;
            this.$emit('orderedBurger', { name:   this.burger.name,
                                          amount: this.amountOrdered
                                        }
                                        );
        },
        dec: function () {
          this.amountOrdered -=1;
            this.$emit('orderedBurger', { name:   this.burger.name,
                                          amount: this.amountOrdered
                                        }
                                        );
        }
      }
  }
  </script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .burgers {
      border: 5px dotted #ff9900;
      background-color: black;
      color: white;
  }

.burger img {
   height: 320px;
   width: 320px;
   padding: 20px
}
.arrows img {
    height: 10px;
    width: 10px;
}

  .space {
      margin: 10px;
  }
  </style>
  