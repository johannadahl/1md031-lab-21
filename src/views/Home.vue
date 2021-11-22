<template>
  
 


        <main>
            <div class="wrapper">
                  <h1 class='headerText'>
                      welcome to Jo's Burgers
                  </h1>
              <div class= "header"> 
                  <div class = "HeaderOverLay">
                </div>
            </div>

  <div class= "menu" id = "hamburger">

          <div class = "menuHeader">
             <h2>Select a burger</h2>
                <p> Every burger comes with sallad and fries. If you have any allergies please let us know!  </p>
        </div>

<div class= "burgerWrapper">
    
    

<Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
</div>
</div>
</div>


    <section class = "costumer" id = "cost">
        <div class = "firstCost">
            <h3>Costumer information:</h3>


            <form>
            <p>
                <label for="Full name">Full name</label><br>
                <input class= "enter" type="text" id="full name" v-model="fn" required="required" placeholder="Full name">
            </p>

            <p>
                <label for="E-mail">Email</label><br>
                <input class= "enter" type="text" id="email adress" v-model="em" placeholder="Email-adress">
            </p>



            <p>
                <label for="payment">Payment option</label>
                <select  v-model="pay">
                    <option checked>Swish</option>
                    <option>Faktura</option>
                    <option>Klarna</option>
                    <option>Kort</option>
                </select>
            </p>

            <p>Choose your gender:</p>

                <div>
                    <input type="radio" v-model="gender" value="female" checked>
                    <label for="huey">Female</label>
                </div>
        
                <div>
                    <input type="radio" v-model="gender" value="male">
                    <label for="dewey">Male</label>
                </div>
                
                <div>
                    <input type="radio" id="louie" v-model="gender" value="other">
                    <label for="louie">Other</label>
                </div>
              </form>


                <p> Please indicate point of delivery:</p>
                <div class ="mapdiv">
                    <div id="map" v-on:click="setLocation">
                      <div id = "dots">
                        <div v-bind:style="{left:location.x + 'px', top:location.y + 'px'}">
                      T
                        </div>
                    </div>
                  </div>
                  </div>

                <div>
                    <button class ="button" v-on:click="getButton">
                        Place order
                    </button>
                </div>

        </div>

            <div class = "neonSign">
                <h1 class = "neonText"> Enjoy your burger</h1>
            </div>
        
 
    </section>



    <footer> </footer>
          <hr>
              <div class = "footer">
                  <p class = "copyRight">  © johannas burger corp </p>

                  <div class= "socialMedia"></div>
              </div>  
             
</main>


</template>

<script>

import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'
const socket = io();


/*
function MenuItem(burger, Kcal, url, gluten,lactose) {
  this.name=burger;
  this.Kcal = Kcal;
  this.url = url;
  this.gluten = gluten;
  this.lactose = lactose;
  
}

const burgerOne = new MenuItem('Cheezy Breezy Burger', '500', 
                  "https://www.foodrepublic.com/wp-content/uploads/2012/03/033_FR11785.jpg", 
                  true, true)

const burgerTwo = new MenuItem( 'Veggie Burger', '700', "https://img.koket.se/standard-mega/hamburgare-med-hembakat-hamburgerbrod.jpg", 
               true, false)

const burgerThree = new MenuItem('Chicked Wicked Heaven', '900', 
                "https://images.immediate.co.uk/production/volatile/sites/2/2017/06/Butchies-2.jpg?quality=90&resize=768%2C574", 
                false, true )


const hamburgers = [burgerOne, burgerTwo,burgerThree];

console.log(hamburgers) */






export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers:  menu,
      gender: "Female",
      pay: "Swish",
      orderedBurger:{},
      location: { x: 0,
            y: 0
          },
      orderNumber: 0
               
    }
  },
  methods: {

    
    getOrderNumber: function () {
      /*return  Math.floor(Math.random()*100000);*/
      return this.orderNumber= this.orderNumber+1;
      
       
    },
  

    /* addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top}; */
                              
    /*getButton(){
      console.log(this.fn)
      console.log(this.em)
      console.log(this.sn)
      console.log(this.hn)
      console.log(this.pay)
      console.log(this.gender)
      console.log(this.orderedBurger)

    }*/
    
    
        addToOrder: function (event) {
  this.orderedBurger[event.name] = event.amount;
  },

      getButton: function(){
        socket.emit("addOrder", {
          orderId: this.getOrderNumber(),
          details:{x: this.location.x, y: this.location.y}, orderItems: this.orderedBurger,
        form: {fn: this.fn, em: this.em, gender: this.gender,pay: this.pay},
        }

      )} ,

        setLocation: function(event){
          var offset = {x: event.currentTarget.getBoundingClientRect().left, y: event.currentTarget.getBoundingClientRect().top};
      this.location={x: event.clientX - 10 - offset.x, y: event.clientY - 10 - offset.y}




      }

  }

    
}
</script>




<style>

  #map {
    margin-top:30px;
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    z-index:10;
    
  }
  .mapdiv{
    height:400px;
    width:600px;
    overflow:scroll;
  }


@import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
@import url("https://fonts.googleapis.com/css?family=Exo+2:200i");



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
    margin-left: 80px;
    margin-right: 80px;
    
 }

 .button:hover {
    cursor:pointer;
    background-color: rgb(10, 97, 10);
    color: white;
    border: none;
    position: relative;

 }
.button {
    padding-top: 1px;
    margin-top: 30px;
    font-family: 'Courier New', Courier, monospace;
    font-weight: bold;
    height: 30px;
    width: 140px;
    transition: 200ms;
}


.header {
    background-image: url(https://thatsup.se/content/img/article/11/apr/uppsalas-b%C3%A4sta-barer.jpg);
    min-width: 90vw;
    min-height: 50vh;
    background-position:30% ;
}

.HeaderOverLay{
    min-height: 400px;
    opacity: 70%;
    background-color: #000000;
}

.headerText{
    font-family: 'Exo 2', sans-serif;
    z-index: 4;
    position: absolute;
    top: 30%;
    left:50%;
    transform: translate(-50%,-50%);
    text-transform: uppercase;
    color: rgb(226, 23, 40);
    overflow:hidden;

    text-align:center;
    padding-top:50px;
    width: 100%;
    height:400px;


    font-size: 90pt;
    font-weight: 200;
    font-style: italic;
    color: #fff;
    text-shadow:
    0 0 7px rgb(253, 117, 67),
    0 0 10px rgb(247, 82, 22),
    0 0 21px #f40,
    0 0 42px #f40,
    0 0 82px #f40;
    padding: 0rem 6rem 5.5rem;
    border-radius: 2rem;
    text-transform: uppercase;  
}

.burger{
    display: inline-block;
    
    margin:auto;
    background-color: none;
    border:4px dotted rgb(224, 250, 230);
    border-top-left-radius: 6px;
    border-top-right-radius: 6px;
    background-color: #050805;
}


.wrapper {
    display: grid;
    grid-row-gap: 10px;
    background-color: rgb(0, 0, 0);
    grid-template-columns:auto;

}



#cost {
    color: black;
    padding-top: 10px;
    padding-left: 20px;
    padding-bottom: 10px;
    margin-top: 10px;

}
.costumer{
  
    background: linear-gradient(to right, #ecb641ef, rgb(0, 0, 0));
    min-height: 500px;
    padding-top: 10px;
    border: 6px  solid #e9e9e9;
    margin-left: 75px;
    margin-right: 70px;
    margin-bottom: 40px;
    display: grid;
    grid-template-columns: 50% 50%;
    
}

#allergies{
    font-family: "comic sans";
    font-weight: bold;
    font-size:16px;
}



.burgerWrapper{
    display:grid;
    grid-template-columns: 26% 26% 26%;
    padding-left: 30px;
    grid-gap: 100px;
    padding-bottom: 40px;
    padding-top: 20px;
}

.menuHeader{
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


.copyRight{

    color:white;
    grid-column: 1;
}
.socialMedia{

    background-image: url("https://musicbiz.org/wp-content/uploads/2018/05/social-1.jpg");
    min-height: 90px;
    min-width: 70px;
    background-position: 50%;
    background-size: 80%;
    background-repeat: no-repeat;
    grid-column: 4;
    
    
}

.footer{

    margin-left: 20px;
    display: grid;
    grid-template-columns: 25% 25% 25% 25%;

}


.neonSign {
margin-top: 120px;
margin-right:60px;
grid-column: 2;
font-size: 40px;

  color: #fff;
  font-style: italic;
    color: #fff;
    text-shadow:
    0 0 7px rgb(253, 117, 67),
    0 0 10px rgb(247, 82, 22),
    0 0 21px #f40,
    0 0 42px #f40,
    0 0 82px #f40;
    
    width: 500px;
    height: 150px;
}
.neonSign:hover{
    color: #fff;
    text-shadow:
    0 0 7px rgb(65, 253, 65),
    0 0 10px rgb(130, 252, 16),
    0 0 21px rgb(130, 252, 16),
    0 0 42px rgb(130, 252, 16),
    0 0 82px rgb(130, 252, 16);

}

#dots div {
    position: relative;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background: url(/img/polacks.jpg);
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }

 


</style>





