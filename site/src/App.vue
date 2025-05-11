<script setup>
import { ref, computed } from 'vue'

const paginaAtual = ref("home");

function irParaCarrinho() {
paginaAtual.value = "carrinho";
}

const searchQuery = ref('')

const produtos = [
{ id: 1, titulo: 'Marfim', resenha: 'Estruturado com cauda volumosa e decote em coração', preco: 12, capa: '/img/vestido marfim.jpg' },
{ id: 2, titulo: 'Verano', resenha: 'Leve e fluido com tecido esvoaçante, mangas soltas e detalhes rendados.', preco: 9.5, capa: '/img/Verano.jpg' },
{ id: 3, titulo: 'Seraphine', resenha: 'Corte princesa com saia volumosa e corpo bordado com pedrarias.', preco: 18, capa: '/img/Seraphine.jpg' },
{ id: 4, titulo: 'Or Blanc', resenha: 'Com brilho dourado discreto e corte ajustado.', preco: 14, capa: '/img/Or Blanc.jpg' },
{ id: 5, titulo: 'Lumière', resenha: 'Delicado e iluminado com detalhes cintilantes que refletem a luz.', preco: 13.5, capa: '/img/Lumière.jpg' },
{ id: 6, titulo: 'La Reine', resenha: 'Majestoso de corte clássico com saia ampla e sem mangas.', preco: 20, capa: '/img/La Reine.jpg' },
{ id: 7, titulo: 'La Promesse', resenha: 'Delicado com caimento suave e iluminado, com bordados sutis.', preco: 10.5, capa: '/img/La Promesse.jpg' },
{ id: 8, titulo: 'Belladona', resenha: 'Sensual com corte sereia, rendas marcantes e aplicação de flores.', preco: 16, capa: '/img/Belladonna.jpg' }
]

const filteredItems = computed(() =>
produtos.filter(item =>
item.titulo.toLowerCase().includes(searchQuery.value.toLowerCase())
)
)

const carrinho = ref([])

function adicionarAoCarrinho(produto) {
const item = carrinho.value.find(item => item.produtos.id === produto.id);
if (item) {
item.quantidade++;
} else {
carrinho.value.push({ produtos: produto, quantidade: 1 });
}
}
function removerDoCarrinho (produtos) {
carrinho.value = carrinho.value.filter(item => item.produtos.id !== produtos.id)
}

function aumentarQuantidade (produtos) {
const item = carrinho.value.find(item => item.produtos.id === produtos.id);
if (item) item.quantidade++;
}

function diminuirQuantidade (produtos) {
const item = carrinho.value.find(item => item.produtos.id === produtos.id);
if (item && item.quantidade > 1) {
item.quantidade--;
} else {
removerDoCarrinho(produtos);
}
}
function limparCarrinho () {
carrinho.value = [];
}
function calcularTotal () {
return carrinho.value.reduce((total, item) => {
return total + (item.produtos.preco * item.quantidade);
}, 0);
}

</script>

<template>
<header class="header">
  <nav>
    <img src="/img/logo-site.png" alt="Logo" class="logo" />
      <div class="search-container">
      <input type="text" v-model="searchQuery" placeholder="Pesquisar" />
        <ul v-if="searchQuery" class="search-dropdown">
          <li v-for="item in filteredItems" :key="item.id">{{ item.titulo }}</li>
        </ul>
      </div>
        <ul class="nav-links">
          <li><a href="#">Termos</a></li>
          <li><a href="#">Equipe</a></li>
          <li><a href="#">Envio</a></li>
          <li><a href="#">Devoluções</a></li>
        </ul>
      <div class="icons">
        <a href="#"><span class="mdi mdi-cart" @click.prevent="irParaCarrinho"></span></a>
        <a href="#"><span class="mdi mdi-heart"></span></a>
        <a href="#"><span class="mdi mdi-account"></span></a>
        <a href="#"><span class="mdi mdi-truck"></span></a>
      </div>
  </nav>
</header>
<section class="oferecer">
  <button class="highlight">Vestido destaque</button>
  <h1>La Promesse</h1>
  <p>
    Um vestido de casamento com um toque de elegância. A La Promesse é perfeita para este momento especial, com detalhes que encantam e um caimento que valoriza a silhueta.
  </p>
  <button class="cta">Acessar página do vestido</button>
</section>
<section class="beneficios">
  <ul>
    <li>
      <span class="mdi mdi-truck"></span>
      <p>Frete grátis para SC</p>
    </li>
    <li>
      <span class="mdi mdi-heart"></span>
      <p>Vestidos recomendados</p>
    </li>
    <li>
      <span class="mdi mdi-star"></span>
      <p>Mais vendidos</p>
    </li>
  </ul>
</section>
<section class="lancamentos" v-if="paginaAtual === 'home'">
  <h3>Lançamentos</h3>
    <div class="produtos-grid">
      <div class="produto-card" v-for="produto in produtos" :key="produto.id">
        <img :src="produto.capa" :alt="produto.titulo" />
          <h4>{{ produto.titulo }}</h4>
            <p>{{ produto.resenha }}</p>
            <p class="preco">R$ {{ produto.preco.toFixed(3) }}</p>
          <button @click="adicionarAoCarrinho(produto)">Adicionar ao carrinho</button>
      </div>
    </div>
</section>
<section class="carrinho" v-if="paginaAtual === 'carrinho'">
  <h3>Carrinho</h3>
    <div v-if="carrinho.length === 0">Carrinho vazio</div>
      <div v-else>
        <ul>
          <li v-for="item in carrinho" :key="item.produtos.id">
            {{ item.produtos.titulo }} - R$ {{ item.produtos.preco.toFixed(2) }} x {{ item.quantidade }}
            <button @click="aumentarQuantidade(item.produtos)">+</button>
            <button @click="diminuirQuantidade(item.produtos)">-</button>
            <button @click="removerDoCarrinho(item.produtos)">Remover</button>
          </li>
        </ul>
          <p>Total: R$ {{ calcularTotal().toFixed(2) }}</p>
          <button @click="limparCarrinho">Limpar carrinho</button>
    </div>
</section>
</template>

<style scoped>
.header {
  background: #fff;
  border-bottom: 1px solid #ddd;
  font-family: "Montserrat", sans-serif;
  margin: 0 10vw 3vw 10vw;
}

.icons span {
  color: #584c4c;
}

nav {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

.logo {
  width: 8%;
}

.search-container {
  position: relative;
}

.search-container input {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.search-dropdown {
  position: absolute;
  top: 2.5rem;
  background: white;
  border: 1px solid #ccc;
  width: 100%;
  z-index: 10;
  list-style: none;
  padding: 0.5rem;
}

.nav-links {
  list-style: none;
  display: flex;
  gap: 1rem;
}

.nav-links a {
  text-decoration: none;
  color: #333;
}

.icons span {
  font-size: 1.5rem;
  margin-left: 0.5rem;
}

.oferecer {
  text-align: center;
  padding: 2rem;
  background: #f9f9f9;
  font-family: "Montserrat", sans-serif;;
}

.highlight {
  background: #eee;
  border: none;
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  font-weight: bold;
}

.cta {
  background: black;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  margin-top: 1rem;
  cursor: pointer;
}

.beneficios ul{
  display: flex;
  justify-content: space-between;
  margin: 0 10vw 0 10vw;
}
.beneficios ul li {
  text-align: center;
}

.beneficios div {
  text-align: center;
  font-size: 1rem;
}

.lancamentos {
  margin: 5vw 2vw 5vw 2vw;
  font-family: "Montserrat", sans-serif;
}
.lancamentos h3 {
 text-align: center;
 font-size: 2rem;
 font-style: italic;
}

.produtos-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 2rem;
  margin: 2vw 5vw 0 5vw;
}

.produto-card {
  background: #fff;
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 1vw;
  text-align: center;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
}

.produto-card h4 {
  font-weight: 500;
  font-size: 1.3rem;
  padding-bottom: 1vw;
}
.produto-card p {
  padding-bottom: 10px;
  text-align: left;
}

.produto-card img {
  width: 100%;
  border-radius: 4px;
  margin-bottom: 0.5rem;
  height: 24vw;
}

.produto-card button {
  border: none;
  border-radius: 5px;
  padding: 8px;
  margin-top: 10px;
  font-weight: 500;
}

.preco {
  font-weight: bold;
  color: #5a4747;
}

</style>