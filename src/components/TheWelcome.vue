
<template>
  <nav id="navbar">
    <div class="nav-container">
      <p class="balance">Ваш баланс: {{ balance }}</p>
      <div class="nav-controls">
        <!--  -->
        <input type="text" id="search" v-model="search_str" placeholder="Поиск по названию или категории">
        <!--  -->
        <select v-model="category_choice">
          <option value="">Все категории</option>
          <option v-for="category in uniqueCategories" :key="category" :value="category">
            {{ category }}
          </option>
        </select>
        <!--  -->
        <div class="sort-buttons">
          <button @click="picked = 'asc'" :class="{ active: picked === 'asc' }">По возрастанию</button>
          <button @click="picked = 'desc'" :class="{ active: picked === 'desc' }">По убыванию</button>
        </div>
      </div>
    </div>
  </nav>
  <!--  -->
  <main>
    <div class="hero-section">
      <img :src="imgLink" class="hero-image"/>
      <h1 class="hero-title">Торговая площадка СуНуХвча:</h1>
    </div>
    <!--  -->
    <div class="products-container">
      <div class="product-card" v-for="(i, index) in filteredAndSortedProducts" :key="i.id">
        <p>Название: {{ i.name }}</p>
        <hr/>
        <p>Категория: {{i.category}}</p>
        <p>Цена: {{ i.price }}</p>
        <p>Количество: {{ i.stock }}</p>
        <button @click="clickBuy(findProductIndex(i.id), i)">купить</button>
        <br/>
        <br/>
      </div>
    </div>
  </main>
</template>


  <script>
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
          category_choice: "",
          picked: "",
          imgLink: "https://i.pinimg.com/236x/1a/88/34/1a8834f8bb9de39116cd71d182b13e77.jpg"
        }
      },
      computed: {
        filteredAndSortedProducts() {
          let result = this.products;
          //////
          if (this.search_str) {
            result = result.filter(product =>
              product.name.toLowerCase().includes(this.search_str.toLowerCase()) ||
              product.category.toLowerCase().includes(this.search_str.toLowerCase())
            );
          }
          //////
          if (this.category_choice) {
            result = result.filter(product => product.category === this.category_choice);
          }
          //////
          if (this.picked === 'asc') {
            result = result.sort((a, b) => a.price - b.price);
          } else if (this.picked === 'desc') {
            result = result.sort((a, b) => b.price - a.price);
          }
          //////
          return result;
        },

        uniqueCategories() {
          const categories = this.products.map(product => product.category);
          return [...new Set(categories)];
        }
      },
      methods:{
        clickBuy(index, item){
          if(item!=null && 
          this.balance >= this.products[index].price &&
          this.products[index].stock > 0){
            this.products[index].stock = this.products[index].stock-1;
            this.balance = this.balance - this.products[index].price;
          }
          else{
            return;
          }
        },
        //////
        findProductIndex(productId) {
          return this.products.findIndex(product => product.id === productId);
        }
      },
      // при загрузке сайта
      mounted(){
      }
    }

  </script>



<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}

#navbar {
  width: 100%;
  background: #0a0e27;
  padding: 1rem;
  margin: 0;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  backdrop-filter: blur(10px); 
}

.nav-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  max-width: 1280px;
  margin: 0 auto;
  gap: 1rem;
}

.balance {
  color: white;
  font-weight: bold;
}

.nav-controls {
  display: flex;
  gap: 1rem;
  align-items: center;
  flex-wrap: wrap;
}

#search {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #ccc;
}

select {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #ccc;
}

.sort-buttons {
  display: flex;
  gap: 0.5rem;
}

.sort-buttons button {
  padding: 0.5rem 1rem;
  border: 1px solid #ccc;
  background: #f0f0f0;
  cursor: pointer;
  border-radius: 4px;
}

.sort-buttons button.active {
  background: #007bff;
  color: white;
}

main {
  width: 100%;
  margin: 0;
  padding: 1rem;
  max-width: 1280px;
  margin: 0 auto;
  padding-top: 100px; 
}

.hero-section {
  text-align: left;
  margin-bottom: 2rem;
}

.hero-image {
  width: 100%;
  max-width: 800px;
  display: block;
  margin-bottom: 1rem;
}

.hero-title {
  color: white;
  font-size: 2rem;
}

.products-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1.5rem;
  width: 100%;
}

.product-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 1rem;
  background: white;
  color: black;
}

.product-card p {
  margin-bottom: 0.5rem;
}

.product-card button {
  background: #2c3e50;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
}

.product-card button:hover {
  background: #1a2530;
}

html, body {
	font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
	background: #0a0e27;
	color: #ffffff;
	overflow-x: hidden;
	margin: 0;
	padding: 0;
}

</style>