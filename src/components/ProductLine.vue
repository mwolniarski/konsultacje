<template>
    <tr>
      <td>
        <select class="form-select" aria-label="Default select example" v-model="productId" @change="updateProductData">
          <option v-for="product in productList" v-bind:key='product.id' v-bind:value='product.id'>{{product.Product_Name + " [" + product.Product_Code + "]"}}</option>
        </select>
      </td>
      <td>{{number}}</td>
      <td>{{line}}</td>
      <td>
        <div class="mb-3">
          <input type="text" class="form-control" value="0">
        </div>
      </td>
      <td>{{id}}</td>
      <td>
        <button type="button" class="btn btn-outline-danger" v-on:click="$emit('deleteEle',id)">Usuń</button>
      </td>
    </tr>
</template>

<script>
// const ZOHO = window.ZOHO;
export default {
  name: 'ProductLine',
  props: {
    productList: [],
    id: Number
  },
  data(){
    return{
      productId: "",
      product: {},
      number: "",
      line: "",
    }
  },
  methods: {
    updateProductData(){
      this.productList.forEach(element => this.productId === element.id ? this.product = element : "");
      this.number = this.product.Product_Code;
      this.line = this.product.Product_line.name;
      this.line = this.product.Product_line.name;
    }
  },
  created() {
  }
}
</script>

