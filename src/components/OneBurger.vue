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


<form>
  <div class="value-button" id="decrease" v-on:click="decreaseValue()" value="Decrease Value">-</div>
  <span id="number">
                  {{amountOrdered}}
  </span>
  <div class="value-button" id="increase" v-on:click="increaseValue()" value="Increase Value">+</div>
</form>
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
increaseValue: function () {
          this.amountOrdered +=1;
            this.$emit('orderedBurger', { name:   this.burger.name,
                                          amount: this.amountOrdered
                                        }
                                        );
},

decreaseValue: function () {
    if (this.amountOrdered > 0)
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
   padding: 20px;
   height: auto;
   max-width: 100%;

}
.arrows img {
    height: 10px;
    width: 10px;
}

  .space {
      margin: 10px;
  }

form {
  width: 300px;
  margin: 0 auto;
  text-align: center;
  padding-top: 50px;
}

.value-button {
  display: inline-block;
  border: 1px solid #ddd;
  margin: 0px;
  width: 40px;
  height: 20px;
  text-align: center;
  vertical-align: middle;
  padding: 11px 0;
  background: #eee;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.value-button:hover {
  cursor: pointer;
}

form #decrease {
  margin-right: -4px;
  border-radius: 8px 0 0 8px;
}

form #increase {
  margin-left: -4px;
  border-radius: 0 8px 8px 0;
}

form #input-wrap {
  margin: 0px;
  padding: 0px;
}

span#number {
  text-align: center;
  border: none;
  border-top: 1px solid #ddd;
  border-bottom: 1px solid #ddd;
  margin: 20px;
  width: 40px;
  height: 40px;
}

input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
}
  </style>
