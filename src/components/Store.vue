<template>

    <div>
    
      <ul class='products-list'>
        <li v-for='item in products' @click='loadModal(item.id)'>
          <h1 class='product-title'>{{item.id}} - {{ item.name }}</h1>
          <b class='d-block'>{{ item.currency }} {{ item.price }}</b>
          <em>{{ item.universe.name }}</em>
          <p>{{ item.shortDescription }}</p>
          <img :src='item.gallery.listing'>
        </li>
      </ul>
      
      <div v-if='modal' class='modal'>
        <button class='close'@click='closeModal()' type='button'>&#215;</button>
        <div>
          <h1 class='product-title'>{{modalData.id}} - {{ modalData.name }}</h1>
          <h2>{{ modalData.subtitle }}</h2>
          <img :src='modalData.gallery.listing'>
          <b class='d-block'>{{modalData.currency}} {{ modalData.price }}</b>
          <p>{{ modalData.description }}</p>
          <ul class='image-list'>
            <li v-for='image in modalData.gallery.items'>
              <img :src='image'>
            </li>
          </ul>
        </div>
      </div>
      
    </div>
    
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'smart',
    data: function () {
      return {
        products: [],
        modal: '',
        modalData: []
      }
    },
    methods: {
      loadData () {
        axios.get('http://smartboxtest.getsandbox.com/products.json')
        .then((response) => {
          this.products = response.data
        })
      },
      loadModal (id) {
        let allProducts = this.products
        allProducts.map((item) => {
          if (item.id === id) {
            this.modalData = item
            this.modal = true
          }
        })
      },
      closeModal () {
        this.modal = false
        this.modalData = ''
      }
    },
    beforeMount () {
      this.loadData()
    }
  }
</script>