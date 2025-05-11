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
{ id: 4, titulo: 'Or Blanc', resenha: 'Simples e elegante, tecido brilhante com cintura alta e corte ajustado.', preco: 14, capa: '/img/Or Blanc.jpg' },
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
  <div>
  <h1>Vestido destaque</h1>
  <h2>La Promesse</h2>
  <p>
    Um vestido de casamento com um toque de elegância. A La Promesse é perfeita para este momento especial, com detalhes que encantam e um caimento que valoriza a silhueta.
  </p>
  <a class="button-vestido" href="produto-card">Acessar página do vestido</a>
  </div>
  <img src="/img/La Promesse.jpg" alt="La Promesse">
</section>

<section class="beneficios">
  <ul>
    <li>
      <span class="mdi mdi-truck"></span>
      <p>Frete grátis para SC</p>
    </li>
    <li class="borda">
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
    <button class="button-carrinho" @click.prevent="irParaCarrinho">Ir para carrinho</button>
</section>
<section class="carrinho" v-if="paginaAtual === 'carrinho'">
  <h3>Carrinho</h3>
    <div v-if="carrinho.length === 0" class="div-carrinho-vazio">
      <p>Seu carrinho está vazio</p>
    </div>
      <div v-else>
        <ul>
          <li v-for="item in carrinho" :key="item.produtos.id">
            <img :src="item.produtos.capa" />
            <div>
              <p>{{ item.produtos.titulo }} - R$ {{ item.produtos.preco.toFixed(3) }} x {{ item.quantidade }}</p>
            <div class="buttons-carrinho">
              <button @click="aumentarQuantidade(item.produtos)">+</button>
              <button @click="diminuirQuantidade(item.produtos)">-</button>
              <button @click="removerDoCarrinho(item.produtos)">Remover</button>
            </div>
            </div>
          </li>
        </ul>
          <div class="div-limpar-carrinho">
            <p>Total: R$ {{ calcularTotal().toFixed(3) }}</p>
            <button @click="limparCarrinho">Limpar carrinho</button>
          </div>

    </div>
    <a href="lancamentos">Voltar para a página inicial</a>
</section>
<footer>
  <div>
  <p>Redes Sociais:</p>
  <ul>
    <li>
      <a href="https://x.com/"><span class="mdi mdi-twitter"></span></a>
    </li>
    <li>
      <a href="https://www.instagram.com/"><span class="mdi mdi-instagram"></span></a>
    </li>
    <li>
      <a href="https://www.facebook.com/"><span class="mdi mdi-facebook"></span></a>
    </li>
  </ul>
</div>
  <p>
    Copyright &copy; 2025, Todos os direitos reservados
  </p>
</footer>
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
  color: #5a4747;
}

.icons span {
  font-size: 1.5rem;
  margin-left: 0.5rem;
}

.oferecer {
  padding: 3vw 15vw 3vw 15vw;
  background: #e2dfdf;
  font-family: "Montserrat", sans-serif;
  display: flex;
}

.oferecer div {
  margin-top: 4vw;
}

.oferecer h1 {
  font-size: 1.7rem;
  font-weight: 500;
  padding-bottom: 1vw;
  color: #5a4747;
}

.oferecer h2 {
  font-size: 1.5rem;
  font-style: italic;
  padding-bottom: 0.5vw;
}

.oferecer p {
  font-size: 1.3rem;
  padding-bottom: 3vw;
  margin: 0 6vw 0 0;
  line-height: 1.5rem;
}

.button-vestido {
  background: #5a4747;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 0.75rem 1.5rem;
  margin-top: 1rem;
  text-decoration: none;
}

.oferecer img {
  width: 80%;
  border-radius: 20px;
  box-shadow: 5px 5px 10px #5a4747;
}

.beneficios {
  font-family: "Montserrat", sans-serif;
  margin: 8vw 0 8vw 0;
}

.beneficios ul li.borda {
  border-right: 1px solid #5a4747;
  border-left: 1px solid #5a4747;
  padding: 0 7vw 0 7vw;
}


.beneficios ul {
  display: flex;
  justify-content: space-between;
  margin: 0 10vw 0 10vw;
}

.beneficios ul li {
  text-align: center;
  border-right: #5a4747;
  font-size: 1.5rem;
}

.beneficios ul li span {
  color: #5a4747;
  font-size: 2.5rem;
}

.lancamentos {
  margin: 0 2vw 5vw 2vw;
  font-family: "Montserrat", sans-serif;
  text-align: center;
}
.lancamentos h3 {
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

.lancamentos button.button-carrinho {
  border: none;
  border-radius: 5px;
  padding: 8px;
  font-weight: bold;
  font-size: 1.3rem;
  margin: 4vw 0 0 0;
  padding: 1vw;
  color: #5a4747;
}

.produto-card h4 {
  font-weight: 500;
  font-size: 1.3rem;
  padding-bottom: 1vw;
}
.produto-card p {
  padding-bottom: 10px;
  text-align: left;
  line-height: 1.5rem;
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
  color: #5a4747;
}

.preco {
  font-weight: bold;
  color: #5a4747;
}

.carrinho {
  margin: 5vw;
  font-family: "Montserrat", sans-serif ;
}
.carrinho h3 {
  text-align: center;
  font-size: 2rem;
  font-style: italic;
  margin-bottom: 3vw;
}

.div-carrinho-vazio p{
  text-align: center;
  font-size: 1.5rem;
  margin-bottom: 1vw;
}

.carrinho a {
  color: #5a4747;
  display: flex;
  justify-content: center;
  font-size: 1.3rem;
}

.carrinho ul li img {
  width: 15%;
  height: 18vw;
}
.carrinho ul li {
  display: flex;
  margin: 0 0 2vw 5vw;
}

.carrinho ul li button {
  border: none;
  margin: 1vw 0 1vw 1vw;
  padding: 0.3vw 1.5vw 0.3vw 1.5vw;
  font-size: 1.2rem;
  border-radius: 10px;
  color: #5a4747;
  font-weight: bold;
 }

 .carrinho ul li p {
  font-size: 1.3rem;
  margin-left: 1vw;
 }

 .carrinho .div-limpar-carrinho p {
  font-size: 1.3rem;
  font-weight: 500;
  margin: 0 0 1vw 5vw;
 }

 .carrinho button {
  border: none;
  padding: 0.5vw 2vw 0.5vw 2vw;
  font-size: 1rem;
  border-radius: 10px;
  color: #5a4747;
  font-weight: bold;
  margin-left: 5vw;
 }

footer {
  background-color: #e2dfdf;
  padding: 5vw;
  text-align: center;
  font-family: "Montserrat", sans-serif;
}

footer div p {
  font-size: 1.3rem;
}

footer div ul {
  display: flex;
  justify-content: center;
  padding: 0.5vw 0 2vw 0;
}

footer div ul li span {
  color: #584c4c;
  font-size: 2rem;

}
footer li {
  padding: 0 0.5vw 0 0.5vw;
}
</style>