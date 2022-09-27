<template>
  <div>
    <b-table-simple responsive>
      <b-thead>
        <b-tr>
          <b-th>ID </b-th>
          <b-th>Product Name </b-th>
          <b-th>Product Price</b-th>
          <b-th>Product Description </b-th>
          <b-th>Product Image </b-th>
          <b-th>
            <b-button variant="warning" @click="openAddModal">ADD❤️</b-button>
          </b-th>
        </b-tr>
      </b-thead>
      <b-tbody>
        <b-tr v-for="product in products" :key="product._id">
          <b-td>{{ product.id }}</b-td>
          <b-td>{{ product.productName }}</b-td>
          <b-td>{{ product.productPrice }}</b-td>
          <b-td>{{ product.productDescription }}</b-td>
          <b-td>
            <img :src="product.productImageURL" width="220px" height="160px" />
          </b-td>
          <b-td>
            <b-button
              variant="primary"
              class="mx-2"
              @click="getSelectedProduct(product)"
              >EDIT</b-button
            >
            <b-button variant="danger" @click="deleteitem(product.id)"
              >DELETE</b-button
            ></b-td
          >
        </b-tr>
      </b-tbody>
    </b-table-simple>
    <add-product
      :showHideAddModal="showHideAddModal"
      @closeAddModal="closeAddModal"
      :products="products"
    />

    <edit-product
      :selectedProduct="selectedProduct"
      :showHideEditModal="showHideEditModal"
      @closeEditModal="closeEditModal"
    />
  </div>
</template>

<script>
import AddProduct from "./Add-Product.vue";
import EditProduct from "./Edit-Product.vue";
export default {
  components: { AddProduct, EditProduct },
  data() {
    return {
      products: [
        {
          id: 1,
          productName: "Audi R8",
          productPrice: 30000000,
          productDescription: "Good product",
          productImageURL:
            "https://imgd.aeplcdn.com/1056x594/cw/ec/21724/Audi-R8-Right-Front-Three-Quarter-66713.jpg?v=201711021421&q=75&wm=1",
        },
        {
          id: 2,
          productName: "smart watch",
          productPrice: 2000,
          productDescription: "Noise waterproof",
          productImageURL:
            "https://encrypted-tbn0.gstatic.com/shopping?q=tbn:ANd9GcRNaFTikHdmmnujKVg08SDtAWk1buqJfeVNkWDlWHybY16cVgWdXlpgOsXce7w5vIPN1AQBxuqmwWh778KavBZ4739duMa4s_A0fWJ1diw0gPYl3x7NkXR_&usqp=CAE",
        },
        {
          id: 3,
          productName: "Mobile",
          productPrice: 300000,
          productDescription: "Very Good product",
          productImageURL:
            "data:image/png;base64,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",
        },
      ],
      showHideAddModal: false,
      showHideEditModal: false,
    };
  },
  methods: {
    // open add modal
    openAddModal() {
      this.showHideAddModal = true;
    },
    //close add modal
    closeAddModal() {
      this.showHideAddModal = false;
    },
    // open edit modal
    openEditModal() {
      this.showHideEditModal = true;
    },
    //close edit modal
    closeEditModal() {
      this.showHideEditModal = false;
    },
    //getting the selected product on click of edit
    getSelectedProduct(prod) {
      this.selectedProduct = prod;
      this.openEditModal();
      console.log("selected prod", prod);
    },
    deleteitem(ID) {
      let tempProduct = this.products;
      let productid = ID;
      console.log(productid);
      console.log(tempProduct);
      tempProduct = tempProduct.filter((item) => {
        return item.id != productid;
      });
      console.log(tempProduct);
      this.products = tempProduct;
    },
  },
};
</script>

<style></style>
