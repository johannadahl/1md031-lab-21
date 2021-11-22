<template>


<figure>
  <div id = "firstburger2" class = "burger firstburg">
        <h3 class = burgerTitle> {{burger.name}}</h3>
        <img class="borgirImg" v-bind:src= "burger.url" alt="Span" title="Burger 2" style=width:265px;>
            <ul>
                <li>{{burger.Kcal}} kCal</li>
                 <span v-if="burger.lactose==true"> <li> <span id = "allergies">Contains lactose </span> </li> </span>
                 <span v-if="burger.gluten==true"> <li>  <span id = "allergies">Contains gluten </span> </li> </span>
            </ul>
             <p class= "amountHam"> Amount ordered: {{ amountOrdered }}</p>
            <button class ="buttonHam" v-on:click="plusCount">
                        +
                    </button>
              

            <button class ="amoutHam" v-on:click="minusCount">
                        -
            </button>
                    
    </div>
    </figure>

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

plusCount(){
this.amountOrdered =this.amountOrdered+1
this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
},

minusCount(){
  if (this.amountOrdered>0)
  this.amountOrdered=this.amountOrdered-1
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

body {
    font-family: 'Exo 2', sans-serif;
    margin: 0px 0px 0px;
    background-color: rgb(0, 0, 0);
    overflow-x: hidden;
}

 #hamburger {
    text-transform: uppercase;
    background-color:#040704;
    color: white;
    border: 6px  dotted;
    margin-left: 60px;
    margin-right: 60px;
 }
.burger{
    display: inline-block;
    font-family: 'Exo 2', sans-serif;
    margin:auto;
    Color: white;
    background-color: none;
    border:4px dotted rgb(224, 250, 230);
    border-top-left-radius: 6px;
    border-top-right-radius: 6px;
    background-color: #050805;
}
    #allergies{
    font-family: 'Exo 2', sans-serif;
    font-weight: bold;
    font-size:16px;
    color:white;
}

.select{
    font-family: IDAHO_Demo_V2_Conrad_Garner;
    font-size: 24px;
    background-color: #030503;
    
    position: relative;
    margin-top:0;
    padding-left: 15px;
    top:0;
    border:dotted transparent;
}

.burgerTitle{
    text-shadow:
    0 0 7px #fff,
    0 0 10px #fff,
    0 0 21px #fff,
    0 0 42px #0fa,
    0 0 82px #0fa;
    text-align: center;
}
.amountHam{
  text-align:center;
}
.buttonHam{
  
  margin-left:110px;
  margin-top:-5px;
}
 

</style>
