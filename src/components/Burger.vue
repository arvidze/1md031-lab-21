<template>
  <div>
    <h3 v-bind:key=burger.name> {{ burger.name }}</h3>
    <img v-bind:src="burger.picture" alt="Burger" style="width: 300px;height:200px">
    <ul>
      <li v-bind:key=burger.kCal> <span>{{ burger.kCal }}</span> Calories</li>
      <li v-if="burger.lactose">Contains <span class="lactose">Lactose</span></li>
      <li v-else>Does not contain <span class="lactose">Lactose</span></li>
      <li v-if="burger.gluten">Contains <span class="gluten">Gluten</span></li>
      <li v-else>Does not contain <span class="gluten">Gluten</span></li>






    </ul>
    <button type="button" v-on:click="addBurger">
      +
    </button>
    <button type="button" v-on:click="removeBurger">
      -
    </button>

    <p>Amount: {{ amountOrdered }} </p>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
  addBurger: function () {
    this.amountOrdered++
    this.$emit('orderedBurger', { name:   this.burger.name,
          amount: this.amountOrdered
        }
    );
  },
  removeBurger: function () {
      this.amountOrdered--
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
button{
  height:50px;
  width: 50px;
}
.gluten {
  font-weight: bold;
}
.lactose {
  font-weight: bold;
}
</style>
