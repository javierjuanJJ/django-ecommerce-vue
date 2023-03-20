<template>
  <div className="home">
    <section className="hero is-medium is-dark mb-6">
      <div className="hero-body has-text-centered">
        <p className="title mb-6">
          Welcome to Djacket
        </p>
        <p className="subtitle">
          The best jacket store online
        </p>
      </div>
    </section>

    <div className="columns is-multiline">
      <div className="column is-12">
        <h2 className="is-size-2 has-text-centered">Latest products</h2>
      </div>

      <ProductBox
          v-for="product in latestProducts"
          v-bind:key="product.id"
          v-bind:product="product"/>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getLatestProducts()

    document.title = 'Home | Djackets'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
          .get('/api/v1/latest-products/')
          .then(response => {
            this.latestProducts = response.data
          })
          .catch(error => {
            console.log(error)
          })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>