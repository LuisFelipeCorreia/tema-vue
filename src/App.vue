<script setup>
import { ref, computed } from 'vue'

const cursos = [
  { id: 1, titulo: 'React Moderno com Hooks', categoria: 'Front-end', nivel: 'Intermediário', carga: '24h' },
  { id: 2, titulo: 'APIs RESTful com Node.js e Express', categoria: 'Back-end', nivel: 'Intermediário', carga: '30h' },
  { id: 3, titulo: 'TypeScript na Prática', categoria: 'Front-end', nivel: 'Básico', carga: '16h' },
  { id: 4, titulo: 'Docker e Deploy em Nuvem', categoria: 'DevOps', nivel: 'Avançado', carga: '20h' },
  { id: 5, titulo: 'Banco de Dados PostgreSQL', categoria: 'Back-end', nivel: 'Básico', carga: '18h' },
  { id: 6, titulo: 'Arquitetura CSS e Tailwind', categoria: 'Front-end', nivel: 'Básico', carga: '12h' }
]

const busca = ref('')
const categoria = ref('Todos')
const favoritos = ref([1])

const alternarFavorito = (id) => {
  if (favoritos.value.includes(id)) {
    favoritos.value = favoritos.value.filter((item) => item !== id)
  } else {
    favoritos.value.push(id)
  }
}

const cursosFiltrados = computed(() => {
  return cursos.filter((curso) => {
    const bateCategoria = categoria.value === 'Todos' || curso.categoria === categoria.value
    const bateBusca = curso.titulo.toLowerCase().includes(busca.value.toLowerCase())
    return bateCategoria && bateBusca
  })
})
</script>

<template>
  <div class="app">
    <header class="topo">
      <div>
        <span class="tag">Projeto 02 • Vue 3</span>
        <h1>Catálogo de Cursos Tech</h1>
      </div>
      <div class="fav-badge">★ Favoritos: {{ favoritos.length }}</div>
    </header>

    <div class="controles">
      <input v-model="busca" type="text" placeholder="Buscar curso..." />
      <div class="filtros">
        <button
          v-for="cat in ['Todos', 'Front-end', 'Back-end', 'DevOps']"
          :key="cat"
          :class="{ ativo: categoria === cat }"
          @click="categoria = cat"
        >
          {{ cat }}
        </button>
      </div>
    </div>

    <main class="grid">
      <article v-for="curso in cursosFiltrados" :key="curso.id" class="card">
        <div class="card-top">
          <span class="cat">{{ curso.categoria }}</span>
          <span class="carga">{{ curso.carga }}</span>
        </div>
        <h2>{{ curso.titulo }}</h2>
        <p class="nivel">Nível: {{ curso.nivel }}</p>
        <button
          class="btn-fav"
          :class="{ favoritado: favoritos.includes(curso.id) }"
          @click="alternarFavorito(curso.id)"
        >
          {{ favoritos.includes(curso.id) ? '★ Favoritado' : '☆ Favoritar' }}
        </button>
      </article>
    </main>
  </div>
</template>

<style>
* { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, sans-serif; }
body { background: #0f172a; color: #f8fafc; }
.app { max-width: 960px; margin: 0 auto; padding: 32px 20px; }
.topo { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 16px; margin-bottom: 24px; }
.tag { background: #059669; color: #d1fae5; padding: 4px 10px; border-radius: 999px; font-size: 0.8rem; font-weight: 600; }
h1 { font-size: 1.8rem; margin-top: 8px; }
.fav-badge { background: #1e293b; border: 1px solid #334155; padding: 10px 16px; border-radius: 10px; font-weight: 600; color: #facc15; }
.controles { display: flex; flex-direction: column; gap: 12px; margin-bottom: 24px; }
.controles input { padding: 12px 14px; border-radius: 8px; border: 1px solid #334155; background: #1e293b; color: #fff; font-size: 1rem; }
.filtros { display: flex; gap: 8px; flex-wrap: wrap; }
.filtros button { background: #1e293b; color: #cbd5e1; border: 1px solid #334155; padding: 8px 14px; border-radius: 8px; cursor: pointer; font-weight: 600; }
.filtros button.ativo { background: #10b981; color: #0f172a; border-color: #10b981; }
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 16px; }
.card { background: #1e293b; border: 1px solid #334155; border-radius: 12px; padding: 20px; display: flex; flex-direction: column; justify-content: space-between; gap: 12px; }
.card-top { display: flex; justify-content: space-between; font-size: 0.8rem; color: #94a3b8; }
.cat { color: #34d399; font-weight: 600; }
.card h2 { font-size: 1.1rem; line-height: 1.4; }
.nivel { font-size: 0.85rem; color: #94a3b8; }
.btn-fav { background: #334155; color: #fff; border: none; padding: 10px; border-radius: 8px; cursor: pointer; font-weight: 600; }
.btn-fav.favoritado { background: #eab308; color: #0f172a; }
</style>