<template>
  <header class="format">
    <div id="container">
      <img id="headerimage" src="https://www.cityworksrestaurant.com/pittsburgh/wp-content/uploads/sites/7/2018/02/header-food-burgers-beer-1600-300.png">
      <h1 id="headertext"> Welcome to BurgerOnline</h1>
    </div>
  </header>

  <hr>

  <main id="grad">
    <section id="burgerSelection" class="format" >
      <h2 class="format"> Select burger: </h2>
      <p class="format"> This is where you select your burger</p>
      <div class="wrapper">

        <Burger v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
      </div>
    </section>

    <section id="delivery">
      <h2 class="format"> Customer information </h2>
      <p class="format"> This is where you enter information</p>

      <h3 class="format"> Delivery information:</h3>


      <form class="format">
        <p>
          <label for="fullname">Full name:</label><br>
          <input type="text" id="fullname" v-model="fn" required="required" placeholder="First and last name">

        </p>
        <p>
          <label for="email">Email-adress:</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        </p>

        <p> Pinpoint your location for delivery: </p>
        <div id="mapcontainer">

          <div id="map" v-on:click="setLocation">

            <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
              T
            </div>
          </div>
        </div>

        <div>
          <label for="payment">Payment method:</label><br>
          <select id="payment" v-model="rcp">
            <option selected="selected">Credit card</option>
            <option>Cash</option>
            <option>Bitcoin</option>
            <option>Swish</option>
          </select>

        </div>
        <div>
          <h4> Gender:</h4>
          <input type="radio" id="contactChoice1"
                 v-model="gender" value="male">
          <label for="contactChoice1">Male</label>
          <br>
          <input type="radio" id="contactChoice2"
                 v-model="gender" value="female">
          <label for="contactChoice2">Female</label>
          <br>
          <input type="radio" id="contactChoice3"
                 v-model="gender" value="nonbinary">
          <label for="contactChoice3">Non-binary</label>
          <br>
          <input type="radio" id="contactChoice4"
                 v-model="gender" value="nondisclosed">
          <label for="contactChoice4">Non-disclosed</label>
        </div>

        <br>

      </form>
    </section>
    <button type="submit" v-on:click="sendOrder">
      <img src="https://icons.iconarchive.com/icons/google/noto-emoji-food-drink/1024/32382-hamburger-icon.png" style="width:12px" >
      Send Info
    </button>

  </main>
  <hr>
  <footer>
    &copy;Arvid Zetterberg 2021
  </footer>

</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '/Users/arvidzetterberg/Desktop/Labb1P/1md031-lab-21/src/assets/menu.json'

const socket = io();

/* function MenuItem(name, kcal, pic, gluten, lactose) {
  this.name = name;
  this.kCal = kcal;
  this.picture = pic;
  this.gluten = gluten;
  this.lactose = lactose;
}
const burgerArray= [new MenuItem("Fire Burger",800,"https://kbr.com.pk/wp-content/uploads/2020/08/burger.jpg",true,true),
  new MenuItem("Chicken Burger",700,"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRKzJIbszy7fOPbHfOZsRiwiOVv2byQ4GLMJg&usqp=CAU",true,false),
  new MenuItem("Vegan Burger",600,"https://www.thespruceeats.com/thmb/K27YgK9PMkhtll2EqIW6x-Crg-E=/960x0/filters:no_upscale():max_bytes(150000):strip_icc():format(webp)/vegan-mushroom-bean-burger-recipe-3378623-13_preview1-5b241897fa6bcc0036d2c9bf.jpeg",false,false)
]; */

const burgerArray2= menu;


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerArray2,
      fn:"",
      em:"",
      rcp:"",
      gender:"",
      orderedBurgers:{},
      location: { x: -20,
        y: -20
      }
    }

  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    }, /* addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
      this.location={x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y
      }
    },*/
    sendOrder: function () {
      console.log(
          "Full name:",this.fn,
          "Email:",this.em,
          "Payment method:",this.rcp,
          "Gender:",this.gender,
          "Ordered burgers:",this.orderedBurgers);

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y,
          fn:this.fn,
          em:this.em,
          rcp:this.rcp,
          gender:this.gender
        },
        orderItems: [this.orderedBurgers] })


    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};

      this.location={x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y
      }

    }
  }
}
</script>

<style>
template {
  font-family: Helvetica,sans-serif;
  font-size: 110%;
}
section{
  margin: 20px;
}

#burgerSelection{
  margin:20px;
  overflow: auto;
  background-color: black;
  color: white;
  border-style:dashed;
}
#delivery{
  overflow:auto;
  clear:left;
  border-style:dashed;
}

button{
  margin:20px;
}
button:hover {
  background-color:lightgray;
  cursor: pointer;
}

.format
{
  margin:20px
}
#container{
  height: 250px;
  overflow:hidden;
}
#headerimage{
  width:100%;
  height:auto;
  opacity:50%;
}
#headertext{
  position: absolute;
  margin-left: 450px;
  margin-top:-150px;
}
.wrapper {
  display: grid;
  grid-gap:200px;
  padding:20px;
  grid-template-columns: 300px 300px 300px;
  background-color: black;
  color: white;
}
  #map {
    position: relative;
    margin: 0;
    padding: 0;
    background: url(/img/polacks.jpg);
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  #mapcontainer{
    width: 800px;
    height: 450px;
    overflow:scroll;
    margin-bottom: 20px;
    border:groove
  }

</style>
