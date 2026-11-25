
<script setup>

import { ref,computed } from 'vue';


const nome = ref("");

const tamanhos = [
    {
        id: "p",
        label: "Pequeno",
        preco: 25.00
    },
    {
        id: "m",
        label: "Médio",
        preco: 35.00
    },
    {
        id: "g",
        label: "Grande",
        preco: 45.00
    }
]
const sabores = [
    "Peperoni",
    "Maguerita",
    "Baiana"
];

const tamanhoSelecionado = ref(tamanhos[0].id);

const tamanhoEscolhido = computed(() => {
    return tamanhos.find((tamanho) => tamanho.id === tamanhoSelecionado.value);
});

const saboresSelecionados = ref([]);
const quantidade = ref(1);

function adicionarPizza() {
    if (quantidade.value < 10) {
        quantidade.value++
    }
}
function removerPizza() {
    if (quantidade.value > 1) {
        quantidade.value--
    }
}
function enviarPedido() {
    alert("Pedido enviado com sucesso!")
}
</script>

<template>
    <main class="container">
        <header class="header">
            <h1>Pizza <span>Vue</span></h1>
            <p>Monte seu pedido e aproveite!</p>
        </header>

        <div class="content">
            <div class="form">
                <label for="nome">Seu nome</label>
                <input type="text" id="nome" placeholder="Digite seu nome..." v-model="nome" />

                <label for="tamanho">Tamanho da pizza</label>
                <select id="tamanho" v-model="tamanhoSelecionado">
                    <option :value="tamanho.id" v-for="tamanho in tamanhos" :key="tamanho.id">
                        {{ tamanho.label }} - R$ {{ tamanho.preco.toFixed(2) }}
                    </option>
                </select>

                <p class="section-label">Escolha os sabores</p>

                <div class="sabores">
                    <label v-for= "sabor in sabores">
                        <input type="checkbox" :value="sabor" v-model="saboresSelecionados" />
                        <span>{{ sabor }}</span>
                    </label>
                </div>

                <label>Quantidade de pizzas</label>
                <div class="quantidade">
                    <button @click="removerPizza">-</button>
                    <span>{{ quantidade }}</span>
                    <button @click="adicionarPizza">+</button>
                </div>

                <button>Confirmar pedido</button>
            </div>

            <aside class="resumo">
                <h2>Resumo do pedido</h2>

                <div class="resumo-item">
                    <span>Cliente:</span>
                    <span v-if="!nome">Não informado</span>
                    <span v-else>{{ nome }}</span>
                </div>

                <div class="resumo-item">
                    <span>Tamanho:</span>
                    <span>{{ tamanhoEscolhido.label}}</span>
                </div>

                <div class="resumo-item">
                    <span>Sabores:</span>
                    <span v-if="saboresSelecionados.length === 0">Nenhum sabor selecionado</span>
                    <span v-else>{{ saboresSelecionados.join(', ') }}</span>
                </div>

                <div class="resumo-item">
                    <span>Quantidade:</span>
                    <span>{{ quantidade }}</span>
                </div>

                <button @click="enviarPedido">Confirmar pedido</button>

                <div class="total">
                    <span>Total:</span>
                    <strong>R$ {{ (tamanhoEscolhido.preco * quantidade).toFixed(2) }}</strong>
                </div>
            </aside>
        </div>

        <footer class="mensagem">Seu pedido está quase pronto!</footer>
    </main>
</template>

<style>
:root {
    font-family: Arial, sans-serif;
    color: #f5f5f5;
    background: #111318;

    --vue-green: #42b883;
    --vue-dark: #35495e;
    --surface: #1c2028;
    --border: #343b48;
    --muted: #a6adbb;
}

* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    padding: 32px 16px;
}

.container {
    width: 100%;
    max-width: 900px;
    margin: 0 auto;
    padding: 28px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
}

.header {
    text-align: center;
    margin-bottom: 32px;
}

.header h1 {
    margin: 0;
    font-size: 2.5rem;
}

.header h1 span,
.total strong {
    color: var(--vue-green);
}

.header p {
    color: var(--muted);
}

.content {
    display: grid;
    grid-template-columns: 1fr 0.8fr;
    gap: 32px;
}

.form {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.form > label,
.section-label {
    margin-top: 12px;
    font-weight: bold;
}

input,
select {
    width: 100%;
    padding: 12px;
    color: #f5f5f5;
    background: #13161c;
    border: 1px solid var(--border);
    border-radius: 8px;
    font: inherit;
}

input:focus,
select:focus {
    outline: 2px solid var(--vue-green);
    outline-offset: 1px;
}

.sabores {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
}

.checkbox {
    display: flex;
    align-items: center;
    gap: 8px;
    color: var(--muted);
    font-weight: normal;
    cursor: pointer;
}

.checkbox input {
    width: auto;
    accent-color: var(--vue-green);
}

button {
    margin-top: 20px;
    padding: 14px;
    color: #101914;
    background: var(--vue-green);
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
}

button:hover {
    background: #35a875;
}

.resumo {
    padding: 20px;
    background: #15181f;
    border: 1px solid var(--border);
    border-radius: 12px;
}

.resumo h2 {
    margin-top: 0;
    font-size: 1.25rem;
}

.resumo-item,
.total {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 16px;
    padding: 16px 0;
    border-bottom: 1px solid var(--border);
}

.resumo-item span:last-child {
    color: var(--muted);
    text-align: right;
}

.total {
    margin-top: 12px;
    border-bottom: none;
    font-size: 1.25rem;
}

.mensagem {
    margin-top: 28px;
    padding: 16px;
    color: var(--muted);
    background: #15181f;
    border: 1px solid var(--border);
    border-radius: 8px;
}

.quantidade {
    width: 100%;
    display: flex;
    align-items: baseline;
    justify-content: center;
    gap: 16px;
}

.quantidade button {
    width: 100%;
    height: 36px;
    padding: 0;
    border: none;
    border-radius: 8px;
    background: #42b883;
    color: #181818;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
}

.quantidade span {
    min-width: 24px;
    text-align: center;
    font-size: 1.5rem;
    font-weight: bold;
}

@media (max-width: 650px) {
    .container {
        padding: 20px;
    }

    .content {
        grid-template-columns: 1fr;
    }

    .header h1 {
        font-size: 2rem;
    }
}
</style>