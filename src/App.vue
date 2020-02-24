<template>
  <div id="app">
  <div id="textfield">
    <input id="inputfirst" type="text" v-model="productID" placeholder="ID"/>
    <input type="text" v-model="productName" placeholder="Name"/>
    <input type="text" v-model="productDescription" placeholder="Description"/>
    <input type="number" v-model="productPrice" placeholder="Price"/>
    <button @click="addProduct">Add</button>
  </div>

    <ul>
      <div v-for="product of products" :key="product.id" id="controller">
      <li>
      <p>ID: {{product.id}}</p>
      <p>Name: {{product.name}}</p>
      <p>Description: {{product.description}}</p>
      <p>Price: {{product.price}}</p>
      </li> 
      </div>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

const readURL = "https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/read";
const addURL = "https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/create";
/* const deleteURL ="https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/delete/${id}"; */

export default {
  name: 'App',
  data() {
    return {
      products: [
       
      ]
    };
  },
  async created() {
    try{
      const res = await axios.get(readURL);
    this.products = res.data;
    } 
    catch (e) {
      console.error(e);
    }
  },
methods: {
  async addProduct(){
    const res = await axios.post(addURL, {name: this.productName, description: this.productDescription, price: this.productPrice, id: this.productID});
  
    this.products = [...this.products, res.data];

    this.productID = '';
    this.productName = '';
    this.productDescription = '';
    this.productPrice = '';
  },
}

};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background: #ececec;
}

li {
  list-style: none;
  align-items: center;
  align-self: center;
  margin: 20px auto;
  text-align: left; 
}

ul {
  display: flex;  
}

#textfield {
  
  justify-content: center;
  align-content: center;
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  width: 60vw;
}

input {
  margin-left: 5%; 
  height: 50px;
  /* border-radius: 30px; */
  align-content: center;
  justify-content: center;
  background: white;
  border: none;
  font-size: 16px;
  padding-left: 20px;
  padding-right: 20px;
  display: flex;
  
}

#inputfirst {
  border-top-left-radius: 30px;
  border-top-right-radius: 30px;
}

input:focus{
      outline: none;
  }

button {
height: 50px;
width: 95%;
border: none;
background: #2ecc71;
color: white;
border-bottom-left-radius: 30px;
border-bottom-right-radius: 30px;
font-size: 16px;
margin-left: 5%; 
margin-bottom: 100px;
}  

#controller {
  width: fit-content;
  min-width: 20vw;
  margin: 0 auto;
  display: flex;
  flex-direction: row;
  align-content: center;
  align-items: center;
  background: white;
  border-radius: 30px;
  
}

</style>
