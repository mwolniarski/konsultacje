<template>
  <div id="container">
    {{contactId}}
    <button type="button" class="btn btn-outline-primary" @click="createLine">Dodaj</button>
    <table class="table table-hover">
      <thead>
      <tr>
        <th scope="col">Produkt</th>
        <th scope="col">Numer</th>
        <th scope="col">Linia</th>
        <th scope="col">Kwota [zł]</th>
        <th scope="col">Numer linii</th>
        <th scope="col"></th>
      </tr>
      </thead>
      <tbody>
        <ProductLine v-bind:productList="productList" v-for="line in productLine" v-bind:key='line.id' v-bind:id="line.id" @deleteEle="deleteLine"/>
      </tbody>
    </table>
    <button type="button" class="btn btn-outline-primary" @click="save">Zapisz</button>
  </div>
</template>

<script>
import ProductLine from './ProductLine.vue'

const ZOHO = window.ZOHO;
export default {
  name: 'ProductTable',
  props: {
    msg: String
  },
  components: {
    ProductLine
  },
  data(){
    return{
      productList: [],
      productLine: [{id:0}],
      contactId: 0
    }
  },
  methods: {
    createLine(){
      this.productLine.push({id:this.productLine.length});
    },
    deleteLine(id){
      this.productLine = this.productLine.filter(element => element.id !== id);
    },
    save(){
    // tutaj pobieramy dane

      var config={
        Entity:"Contacts",
        APIData:{
          "id": this.contactId
          // Company": "Zylker",
          // "Last_Name": "Peterson"
        },
        Trigger:["workflow"]
      }
      ZOHO.CRM.API.updateRecord(config);
    }
  },
  async created(){
    await ZOHO.embeddedApp.on("PageLoad", (data) => this.contactId = data['EntityId']);
    await ZOHO.embeddedApp.init();

    let tmp = await ZOHO.CRM.API.getAllRecords({Entity:"Products"});
    this.productList = tmp.data;
    console.log(this.productList);
  }
}
</script>

