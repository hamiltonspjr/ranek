<template>
  <section>
    <div v-if="product" class="product">
      <ul class="images" v-if="product.fotos">
        <li v-for="(foto, index) in product.fotos" :key="index">
          <img :src="foto.src" :alt="foto.titulo" />
        </li>
      </ul>
      <div class="info">
        <h1>{{ product.nome }}</h1>
        <p class="price">{{ product.preco | numberPrice }}</p>
        <p class="description">{{ product.descricao }}</p>
        <transition v-if="product.vendido === 'false'" mode="out-in">
          <button class="btn" v-if="!finalize" @click="finalize = true">
            Comprar
          </button>
          <FinalizePurchase v-else :product="product" />
        </transition>
        <button class="btn btn-disabled" v-else disabled>Produto Vendido</button>
      </div>
    </div>
    <PageLoading v-else />
  </section>
</template>

<script>
import { api } from "@/services.js";
import FinalizePurchase from "@/components/FinalizePurchase.vue";
export default {
  name: "Product",
  props: ["id"],
  data() {
    return {
      product: null,
      finalize: false,
    };
  },
  components: {
    FinalizePurchase,
  },
  methods: {
    getProduct() {
      api.get(`/produto/${this.id}`).then((response) => {
        this.product = response.data;
        document.title = this.product.nome;
      });
    },
  },
  created() {
    this.getProduct();
  }
};
</script>

<style scoped>
.product {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 30px;
  max-width: 900px;
  padding: 60px 20px;
  margin: 0 auto;
}
.images {
  grid-row: 1 / 3;
}
.info {
  position: sticky;
  top: 20px;
}
.price {
  color: #e80;
  font-weight: bold;
  font-size: 1.5rem;
  margin-bottom: 40px;
}
.description {
  font-size: 1.2rem;
}

img {
  margin-bottom: 30px;
  box-shadow: 0 4px 8px rgba(30, 60, 90, .2);
  border-radius: 4px;
}
.btn {
  margin-top: 60px;
  width: 200px;
}
@media (max-width: 500px) {
  .product {
    grid-template-columns: 1fr;
  }
  .images {
    grid-row: 2;
  }
  .info {
    position: initial;
  }
}
</style>