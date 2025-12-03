
<template>
  <header>
   <h1>Торговая площадка СуНуХвча:</h1>
   <img :src="imgLink"/>
   <br/>
 </header>
  <main>
    <header>
        <p>Ваш баланс: {{ balance }}</p>
    </header>
    <body>
        <input type="text" id="search" v-model="search_str">

        <select name="" id=""></select>

        <input type="radio" id="asc" v-model="picked"  @change="sortProducts()">По возрастанию</input>
        <input type="radio" id="desc" v-model="picked"  @change="sortProducts()">По уменьшению</input>

      <div class="block" v-for="(i, index) in products">
        <p>Название: {{ i.name }}</p>
        <hr/>
        <p>Категория: {{i.category}}</p>
        <p>Цена: {{ i.price }}</p>
        <p>Количество: {{ i.stock }}</p>
        <button @click="clickBuy(index, i)">купить</button>
        <label>{{ search_str }}</label>
        <br/>
        <br/>
      </div>
    </body>
  </main>
  
</template>


  <script>
  
  //import axios from 'axios';
    export default {
      data(){
        return{
            balance: 6001,
          products: [
              {
                  id: 1,
                  name: "гвоздика",
                  category: "drugs",
                  price: 100,
                  stock: 312
              },  {
                  id: 2,
                  name: "глицин",
                  category: "drugs",
                  price: 320,
                  stock: 32
              }, {
                  id: 3,
                  name: "карачинская",
                  category: "drinks",
                  price: 88,
                  stock: 1112
              }, {
                  id: 4,
                  name: "гвозди",
                  category: "metal",
                  price: 10,
                  stock: 3127
              }, {
                  id: 5,
                  name: "Костя",
                  category: "slave",
                  price: 2,
                  stock: 1
              }
          ],
          search_str: "",
          caterogy_choice: "",
          sort: true,
          imgLink: "https://i.pinimg.com/236x/1a/88/34/1a8834f8bb9de39116cd71d182b13e77.jpg"
        }
      },
      methods:{
        clickBuy(index, item){
            if(item==null){
          return;
        }
        this.products[index].stock = this.products[index].stock-1
        this.balance = this.balance - this.products[index].price
        } ,
        sort(){
            this.products.sort();
        },
        sortedArray() {
            
        }
      },
      // при загрузке сайта
      mounted(){
        this.getBooks();
      }
    }
  
  </script>



<style>
* {
	margin: 0;
}

html, body {
  height: 100%;
  overflow-x: hidden;
}

header{
  display: flex;
  justify-content: center;
}

body {
  display: flex;
  flex-direction: column;
  font-family: Arial, sans-serif;
  justify-content: left;
}

footer {
  padding-left: 1%;
  margin: 10px;
  position: fixed;
  bottom: 0;
  width: 100%;
  overflow: hidden;
}

h1 {
  width:100%;
  display: flex;
  justify-content: left;
  margin-left: 1%;
  font-family:'Merienda';
  font-size: 56px;
  font-weight: bold;
}

img {
  display: flex;
  
}

.block{
  width: 100%;
}
</style>