<template>
<h1>Informações dos Produtos Atualizados desde 00:00</h1>
  <div class="catalog">
    <div v-for="(product, index) in products" :key="index" class="product-card">
      <h1>{{ product.descricao }}</h1>
      <img :src="product.imagemPrincipal?.url" :alt="product.descricao" />
      <p><strong>Marca:</strong> {{ product.marca?.nome }}</p>
      <p><strong>Categoria:</strong> {{ product.categoriaProduto?.nome }}</p>
      <p><strong>Categoria Pai:</strong> {{ product.categoriaPai?.nome }}</p>
      <p><strong>Origem:</strong> {{ product.origem?.nome }}</p>
      <p><strong>NCM:</strong> {{ product.classificacaoFiscal?.ncm }}</p>

      <div v-if="product.composicoesLogisticas?.length">
        <h2>Composições Logísticas</h2>
        <ul>
          <li v-for="(composicao, composicaoIndex) in product.composicoesLogisticas" :key="composicaoIndex">
            <div v-for="(nivel, nivelIndex) in composicao.niveis" :key="nivelIndex">
              <p><strong>GTIN:</strong> {{ nivel.gtin }}</p>
              <p><strong>Quantidade:</strong> {{ nivel.quantidade }}</p>
              <p><strong>Altura:</strong> {{ nivel.altura }} {{ nivel.alturaUm?.abrev }}</p>
              <p><strong>Largura:</strong> {{ nivel.largura }} {{ nivel.largauraUm?.abrev }}</p>
              <p><strong>Profundidade:</strong> {{ nivel.profundidade }} {{ nivel.profundidadeUm?.abrev }}</p>
              <p><strong>Peso Líquido:</strong> {{ nivel.pesoLiquido }} {{ nivel.pesoLiquidoUm?.abrev }}</p>
              <p><strong>Peso Bruto:</strong> {{ nivel.pesoBruto }} {{ nivel.pesoBrutoUm?.abrev }}</p>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: []
    };
  },
  created() {
    this.fetchProductData();
  },
  methods: {
    fetchProductData() {
      const url = 'http://localhost:3000/';
      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.products = data; // Supondo que o JSON seja um array de objetos
        })
        .catch(error => console.error('Error fetching product data:', error));
    }
  }
};
</script>

<style scoped>
.catalog {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 20px;
}

.product-card {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 20px;
  width: calc(33.333% - 40px);
  box-sizing: border-box;
}

.product-card h1 {
  font-size: 20px;
  color: #333;
  margin-bottom: 10px;
}

.product-card img {
  max-width: 100%;
  height: auto;
  display: block;
  margin-bottom: 10px;
}

.product-card p {
  font-size: 14px;
  color: #555;
  margin: 5px 0;
}

.product-card h2 {
  font-size: 16px;
  color: #666;
  margin-top: 20px;
  margin-bottom: 10px;
}

.product-card ul {
  list-style-type: none;
  padding: 0;
}

.product-card li {
  margin-bottom: 10px;
}

.product-card strong {
  font-weight: bold;
}
</style>