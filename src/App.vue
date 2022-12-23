<template>

  <navbar/>
  <div class="mx-40">
    <div>{{ saludarUsuario() }}</div>
    <div>
      {{ `${items[0].name.toUpperCase()} hola!`  }}
    </div>

    <input class="px-4 py-2 rounded-md" type="text" v-model="username" placeholder="Set your username"/>
    <p>{{ yourPasswordIs }}</p>

    <input class="px-4 py-2 rounded-md" type="password" v-model="password" placeholder="password"/>
  </div>


  <div class="mx-40 mt-12 max-w-4xl flex flex-col gap-4">
    <article class="grid grid-cols-6 p-4 bg-gray-100 content-center items-center rounded-md" v-for="(item, index) of topItems" :key="item.uuid">
      <div class="pl-12">{{ index }}</div>
      <div>{{ toMayus(item) }}</div>
      <img :src="item.image" :alt="item.name" class="w-24">
      {{ item.quantity }}
      <input class="mr-4 px-4 py-2 rounded-md" type="numbers" v-model="item.quantity" placeholder="0"/>
      {{ item.quantity*item.price}}
    </article>

  </div>

  <div class="mx-40 my-12">
    <div v-if="!limitReached">Añade mínimo 3 artículos</div>
    <div v-else-if="items.length < 5">Añade 5 artículos más para envío gratuito</div>
    <button class="bg-gray-300 rounded-md px-4 py-2" v-else>Confirmar compra {{ numberOfItems }}</button>
  </div>


</template>

<script>
import ITEMS from "./items.json"
import navbar from "./components/navbar.vue"

export default {
  components:{
    navbar
  },

  data (){
    return {
      items: ITEMS,
      username: null,
      errorMessage: "Please set yor username",
      password: null,
      mostrar: false,
    }
  },

  computed: {
    topItems (){
      return this.items.slice(0, 5)
      
    },
    numberOfItems (){
      return this.topItems.length;
    },

    limitReached () {
      return this.items.length > 3
    }


  },

  methods: {
    toMayus(item){
      return item.name.toUpperCase();
    },
    saludarUsuario (){
      if(this.username){
        const english = this.items[0].name.toUpperCase();
        const spanish = this.items[1].name.toLowerCase();

        return english + " " + " " + spanish + " " + this.username;
      } else {
        return `${this.errorMessage} ${this.numberOfItems}`;
      }
    }


  }

}

</script>