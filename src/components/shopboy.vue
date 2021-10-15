<template>
<h1>Shopping Cart</h1>
<div id="app">
  <div v-if="!plants.length" class="cart-checkout">
    <p>Cart is empty.</p>
  </div>
  <div v-if="checkout" class="cart-checkout">
    <p>Thank you for your order!</p>
    <transition-group  name="order-group" tag="div" class="cart-plants" appear>
      <img  v-for="plant in plants"
           @click="checkoutCart"
           :key="plant.id" 
           :src="plant.img" 
           :alt="plant.name"/>
    </transition-group>
  </div>
  <transition v-else name="fade-up" >
    <transition-group  name="plants-group" tag="div" class="plants">
      <div class="plant" v-for="plant in plants" :key="plant.id">
          <img :src="plant.img" :alt="plant.name"/>
          <div class="description">
            <h4>{{plant.name}}</h4>
            <p>Succulent</p>
            <transition name="change-price">
              <span class="price">${{plant.price}}</span>
            </transition>
          </div>
          <div class="quantity">
            <button @click="add(plant)" class="btn add"><i class="fas fa-chevron-up"></i></button>
            <transition name="scale" mode="out-in">
              <span :key="plant.quantity" class="number scale-animation">{{plant.quantity}}</span>
            </transition>
            <button @click="decrease(plant)" class="btn remove"><i class="fas fa-chevron-down"></i></button>
          </div>
      </div>
    </transition-group>
  </transition>
  <div class="checkout">
    <div class="total">Total: 
      <transition name="scale" mode="out-in">
        <span :key="sum" class="scale-animation">{{sum.toFixed(2)}}</span>
      </transition>
    </div>
    <transition name="opacity" mode="out-in">
      <button :disabled="!plants.length" v-if="!checkout" @click="checkoutCart">Checkout</button>
    </transition>
  </div>
</div> 
<a target="_blank" title="instagram/web__addict" href="https://www.instagram.com/web__addict/"><i class="fab fa-instagram"></i></a>
</template>
<script>


export default{
  name:shopboy,
data: {
    plants: [
      {
        id: 1,
        name: "Cactaceae Houseleek",
        img: "https://github.com/OlgaKoplik/CodePen/blob/master/plants/1.png?raw=true",
        price: 3.19,
        quantity: 1,
      },
      {
        id: 2,
        name: "Echeveria purpusorum",
        img: "https://github.com/OlgaKoplik/CodePen/blob/master/plants/2.png?raw=true",
        price: 2.99,
        quantity: 1,
      },
      {
        id: 3,
        name: "Echeveria agavoides",
        img: "https://github.com/OlgaKoplik/CodePen/blob/master/plants/3.png?raw=true",
        price: 3.39,
        quantity: 1,
      }],
    checkout: false,
  },
  computed: {
    sum: function(){
      return this.plants.reduce((sum, plant) => {
        return sum + (plant.price*plant.quantity);
      }, 0)
    }
  },
  methods: {
    add(plant) {
      plant.quantity+=1;
    },
    decrease(plant) {
      if(plant.quantity > 1){
        plant.quantity-=1;
      }else{
        this.remove(plant.id);
      }
    },
    remove(plantIDToRemove) {
      this.plants = this.plants.filter((plant)=>{
        return  plant.id !== plantIDToRemove;
      });
    },
    checkoutCart(){
      this.checkout = !this.checkout;
    }
  },
}



</script>
<style lang="scss" scoped>
@import "@/assets/_shared.scss";
#app{
  position: relative;
  width: 450px;
  height: 645px;
  display: flex;
  background: $primary;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
  border-radius: $border-radius;
  box-shadow: 0px 7px 25px $light-gray;
  transition: $transition;
  .plants{
    position: absolute;
    display: flex;
    flex-direction: column;
    align-items: center;
    width: inherit;
    background: white;
    border-radius: $border-radius;
    box-shadow: 0px 7px 25px $light-gray;
    .plant{
      display: flex;
      width: max-content;
      border-radius: $border-radius;
      transition: $transition;
      padding: 10px;
      margin: 20px 0;
      &:hover{
        box-shadow: 0px 7px 15px $light-gray;
        img{
          transform: rotate(25deg) scale(1.45);
        }
      }
      img{
        width: 100px;
        height: auto;
        border-radius: $border-radius;
        transition: $transition;
      }
      .description{
        width: 180px;
        padding-left: 25px;
        h4{
          margin-top: 5px;
        }
        p{
          color: $gray;
        }
        .price{
          font-weight: 600;
        }
      }
      .quantity{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        margin-left: 35px;
        padding: 0 20px;
        font-size: 18px;
        background: $light-primary;
        border-radius: $border-radius;
        .btn{
          border: none;
          background: none;
          color: $gray;
          transition: $transition;
          cursor: pointer;
            &:hover{
              transform: scale(1.45);
          }
        }
        .number{
          font-weight: 600;
          animation: scale .3s ease;
        }
      }
    }
  }
  
  .checkout{
    position: absolute;
    top: 520px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    .total{
      font-weight: 900;
      display: flex;
      align-items: flex-end;
      font-size: 20px;
      margin-bottom: 20px;
      span{
        font-size: 24px;
        margin-left: 9px;
      }
    }
    button{
      font-size: 18px;
      background: $accent;
      color: #fff;
      font-weight: 700;
      padding: 10px 100px;
      border: none;
      border-radius: $border-radius;
      transition: $transition;
      cursor: pointer;
      &:hover{
        box-shadow: 0px 7px 25px $light-gray;
      }
      &:disabled{
        background-color:$gray;
        cursor: not-allowed;
      }
    }
  }
  .cart-checkout{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 480px;
    margin-top: 20px;
    .cart-plants{
      display: flex;
      height: 300px;
    }
    p{
      font-size: 34px;
      font-weight: 600;
      text-align: center;
      width: 240px;
    }
  }
}
*:focus{
    outline-color: $accent;
  }
body{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  color: $base;
  background-image: linear-gradient(to right top, #fff, #c8e6c9);
}

.fade-up-leave-active {
   transition: .6s all ease;
}
.fade-up-leave{
  transform: translateY(0);
}
.fade-up-enter, .fade-up-leave-to {
  transform: translateY(-500px);
}

.scale-enter-active {
  transition: $transition;
}
.scale-leave-active {
  transition: $transition;
}
.scale-enter, .scale-leave-to{
  transform: scale(1.2);
}

.opacity-leave-active {
   transition: $transition;
}
.opacity-enter, .opacity-leave-to {
  transform: translateY(60px);
  opacity: 0;
}

.plants-group-leave-active {
   transition: $transition;
}
.plants-group-enter, .plants-group-leave-to {
  transform: translateX(200px);
  opacity: 0;
}

.order-group-enter-active {
   transition: 2s all ease
}
.order-group-enter, .order-group-leave-to {
  transform: translateX(-300px);
  opacity: 0;
}


.fa-instagram{
  position: absolute;
  color: $primary;
  top: 3%;
  right: 2%;
  font-size: 38px;
}

.fa-instagram:hover{
  font-size: 42px;
  color: $accent;
  transition: all .1s linear;
  cursor: pointer;
}
</style>
