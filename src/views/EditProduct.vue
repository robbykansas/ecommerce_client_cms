<template>
    <div class="container">
        <div id="edit-page" class="bg-light col mt-5 rounded">
            <form id="edit-form" class="input-margin" @submit.prevent="edit">
                <h1>Edit Form</h1>
                <div class="form-group">
                    <label for="name">Name</label>
                    <input type="text" class="form-control" name="name" id="edit-name" placeholder="name" v-model="editProduct.name">
                </div>
                <div class="form-group">
                    <label for="image">Image</label>
                    <input type="text" class="form-control" name="image" id="edit-image" placeholder="image" v-model="editProduct.image_url">
                </div>
                <div class="form-group">
                    <label for="price">Price</label>
                    <input type="number" class="form-control" name="price" id="edit-price" placeholder="price" v-model="editProduct.price">
                </div>
                <div class="form-group">
                    <label for="stock">Stock</label>
                    <input type="number" class="form-control" name="stock" id="edit-stock" placeholder="stock" v-model="editProduct.stock">
                </div>
                <div class="form-group">
                    <label for="add-category">Category</label>
                    <select class="form-control" id="add-category" v-model="editProduct.category">
                      <option selected disabled>Select Category</option>
                      <SelectCategory
                        v-for="category in listCategories"
                        :key="category.id"
                        :category="category"
                      ></SelectCategory>
                    </select>
                </div>
                <button type="submit" class="btn btn-primary">Submit</button>
            </form>
        </div>
    </div>
</template>

<script>
import SelectCategory from '../components/SelectCategory'
export default {
  name: 'EditProduct',
  data () {
    return {
      editProduct: {
        name: '',
        image_url: '',
        price: '',
        stock: '',
        id: '',
        category: ''
      }
    }
  },
  methods: {
    edit () {
      const editedData = this.editProduct
      this.$store.dispatch('editProduct', editedData)
    },
    fetchCategories () {
      this.$store.dispatch('fetchCategories')
    }
  },
  components: {
    SelectCategory
  },
  computed: {
    listCategories () {
      return this.$store.state.listCategories
    }
  },
  created () {
    this.fetchCategories()
    const id = this.$route.params.id
    this.$store.dispatch('filterId', id)
      .then(response => {
        this.editProduct.name = response.name
        this.editProduct.image_url = response.image_url
        this.editProduct.price = response.price
        this.editProduct.stock = response.stock
        this.editProduct.id = response.id
        this.editProduct.category = response.Categories[0].id
      })
      .catch(err => console.log(err))
  }
}
</script>

<style>

</style>
