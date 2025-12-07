
<template>
  <main>
    <header>
        <p>Ваш баланс: {{ balance }}</p>
    </header>
    <body>
      <input type="text" id="search" v-model="search_str">
      <select v-model="category_choice">
        <option value="">Все категории</option>
        <option v-for="category in uniqueCategories" :key="category" :value="category">
          {{ category }}
        </option>
      </select>
      <div>
        <button @click="picked = 'asc'" :class="{ active: picked === 'asc' }">По возрастанию</button>
        <button @click="picked = 'desc'" :class="{ active: picked === 'desc' }">По убыванию</button>
      </div>
      <div class="block" v-for="(i, index) in filteredAndSortedProducts" :key="i.id">
        <p>Название: {{ i.name }}</p>
        <hr/>
        <p>Категория: {{i.category}}</p>
        <p>Цена: {{ i.price }}</p>
        <p>Количество: {{ i.stock }}</p>
        <button @click="clickBuy(findProductIndex(i.id), i)">купить</button>
        <br/>
        <br/>
      </div>
    </body>
  </main>

</template>


  <script>
    export default {
      data(){
        return{
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
          category_choice: "",
          picked: "",
          imgLink: "https://i.pinimg.com/236x/1a/88/34/1a8834f8bb9de39116cd71d182b13e77.jpg"
        }
      },
      computed: {
        filteredAndSortedProducts() {
          let result = this.products;

          if (this.search_str) {
            result = result.filter(product =>
              product.name.toLowerCase().includes(this.search_str.toLowerCase()) ||
              product.category.toLowerCase().includes(this.search_str.toLowerCase())
            );
          }

          if (this.category_choice) {
            result = result.filter(product => product.category === this.category_choice);
          }

          if (this.picked === 'asc') {
            result = result.sort((a, b) => a.price - b.price);
          } else if (this.picked === 'desc') {
            result = result.sort((a, b) => b.price - a.price);
          }

          return result;
        },

        uniqueCategories() {
          const categories = this.products.map(product => product.category);
          return [...new Set(categories)];
        }
      },
      methods:{
        clickBuy(index, item){
            if(item==null){
          return;
        }
        this.products[index].stock = this.products[index].stock-1
        },
        findProductIndex(productId) {
          return this.products.findIndex(product => product.id === productId);
        }
      },
      mounted(){
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
