<template>
  <div id="app">
  <div id="textfield">
    <input id="inputfirst" type="text" v-model="productID" placeholder="ID"/>
    <input type="text" v-model="productName" placeholder="Name"/>
    <input type="text" v-model="productDescription" placeholder="Description"/>
    <input type="number" v-model="productPrice" placeholder="Price"/>
    <button @click="addProduct" id="add-btn">Add</button>
  </div>

    <ul>
      <div id="centerdiv">
        <div v-for="product of products" :key="product.id" id="controller">

          <div v-if="editProduct === product.id">
            <input v-model="product.name">
            <input v-model="product.description">
            <input v-model="product.price">
            <button v-on:click="updateProduct(product)" id="save-btn"><i class="fa fa-check" aria-hidden="true"></i></button>
          </div>     

          <div v-else>
          <li>
          <p>ID: {{product.id}}</p>
          <p>Name: {{product.name}}</p>
          <p>Description: {{product.description}}</p>
          <p>Price: {{product.price}}</p>
          <button v-on:click="deleteProduct(product.id)" id="del-btn"><i class="fa fa-trash" aria-hidden="true"></i></button>
          <button v-on:click="editProduct = product.id" id="edit-btn"><i class="fa fa-wrench" aria-hidden="true"></i></button>
          </li> 
          </div>
          </div>

      </div>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

const readURL = "https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/read";
const addURL = "https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/create";
/* const deleteURL ="https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/delete/"; */

export default {
  name: 'App',
  data() {
    return {
      products: [],
      editProduct: null,
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

    deleteProduct(id) {
      axios.delete("https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/delete/" + id)
      .then(() => {
        window.location.reload()
      })
    },

    updateProduct(product) {
      fetch("https://us-central1-firstrestapi-25944.cloudfunctions.net/app/api/update/" + product.id,
      {
        body: JSON.stringify(product),
        method: "PUT",
        headers: {
          "content-Type": "application/json",
        },
      })
      .then(() => {
        this.editProduct = null;
      })
    }
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
  margin: 20px; 
  text-align: left; 
  flex-wrap: wrap;
}

ul {
  padding: 0;
  margin: 0 auto;
  display: flex;
  width: 90vw;
  flex-wrap: wrap;
  align-items: center;
  /*justify-content: center;  */ 
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

#controller {
  width: fit-content;
  padding-left: 20px;
  padding-right: 20px;
  margin: 20px;
  display: flex;
  flex-direction: row;
  background: white;
  border-radius: 30px;
  /*padding-left: 30px; */
  flex-wrap: wrap;
}

#add-btn {
height: 50px;
width: 100%;
border: none;
background: #2ecc71;
color: white;
border-bottom-left-radius: 30px;
border-bottom-right-radius: 30px;
font-size: 16px;
margin-bottom: 50px;
}  

#del-btn {
  height: 50px;
  width: 50px;
  border-radius: 15px;
  background: red;
  font-size: 16px;
  color: white;
  border: none;
}

#edit-btn {
  height: 50px;
  width: 150px;
  border-radius: 15px;
  background: #2ecc71;
  font-size: 16px;
  color: white;
  border: none;
  margin-left: 10px;
}

#save-btn {
  height: 50px;
  width: 100%;
  border-radius: 15px;
  background: #2c82c9;
  font-size: 16px;
  color: white;
  border: none;
  margin-left: 10px;
}

#centerdiv {
  display: flex;  
  flex-wrap: wrap;
  align-items: center;
  margin: 0 auto;
  justify-content: center;
  

}

</style>
