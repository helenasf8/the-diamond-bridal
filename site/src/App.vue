<script setup>
export default {
    data() {
        return {
            searchQuery: "",
            items: [
                { id: 1, name: "Marfim" },
                { id: 2, name: "Verano" },
                { id: 3, name: "Seraphine" },
                { id: 4, name: "Or Blanc" },
                { id: 5, name: "Lumière" },
                { id: 6, name: "La Reine" },
                { id: 7, name: "La Promesse" },
                { id: 8, name: "Jardim Secreto" },
                { id: 9, name: "Ipanema" },
                { id: 10, name: "Eternity" },
                { id: 11, name: "Elegant" },
                { id: 12, name: "Belladonna "},
            ]
        }
    }
    computed: {
        filteredItems() {
            return {this.items.filter((item) => 
                item.name.toLowerCase().includes(this.searchQuery.toLowerCase())
            )
        }
    }
}

</script>

<template>
  <header>
    <nav>
        <div>
            <img src="../public/img/logo-site.png" alt="Logo" />
        </div>
        <div>
        <input type="text" v-model="searchQuery" placeholder="Pesquisar" />
        <ul>
            <li v-for="item in filteredItems" :key="item.id">
                {{ item.name }}
            </li>
        </ul>
       </div>
        <ul>
            <li>
                <a href="#">Termos</a>
            </li>
            <li>
                <a href="#">Equipe</a>
            </li>
            <li>
                <a href="#">Envio</a>
            </li>
            <li>
                <a href="#">Devoluções</a>
            </li>
        </ul>
        <ul>
            <li>
                <span class="mdi mdi-cart" />
                <span class="mdi mdi-heart" />
                <span class="mdi mdi-account" />
                <span class="mdi mdi-truck" />
            </li>
        </ul>
    </nav>
  </header>
   <section class="oferecer">
    <button>Vestido destaque</button>

    <h1>La Promesse</h1>
    <p>
        Um vestido de casamento com um toque de elegância. A La Promesse é perfeita para este momento especial, com detalhes que encantam e um caimento que valoriza a silhueta. Feita com tecidos de alta qualidade, ela garante conforto e sofisticação em cada movimento.
    </p>
    <button>Acessar página do vestido</button>
   </section>
   <section class="beneficios">
    <span class="mdi mdi-cart"></span>
    <p>Frete grátis para SC</p>
    <span class="mdi mdi-heart"></span>
    <p>Vestidos recomentados</p>
    <span class="mdi mdi-account"></span>
    <p>Mais vendidos</p>
   </section>
   <section class="Vestidos">
    <h1>Lançamentos</h1>


    </div>

   </section>
</template>

<style scoped>

</style>