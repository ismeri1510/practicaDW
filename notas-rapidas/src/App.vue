<script setup>
import { ref, onMounted } from "vue";

const titulo = ref("");
const contenido = ref("");
const notas = ref([]);

onMounted(() => {
  const notasGuardadas = localStorage.getItem("notas");

  if (notasGuardadas) {
    notas.value = JSON.parse(notasGuardadas);
  }
});

function guardarEnLocalStorage() {
  localStorage.setItem("notas", JSON.stringify(notas.value));
}

function guardarNota() {
  if (titulo.value === "" || contenido.value === "") {
    alert("Debes completar todos los campos");
    return;
  }

  const nuevaNota = {
    id: Date.now(),
    titulo: titulo.value,
    contenido: contenido.value,
  };

  notas.value.push(nuevaNota);

  guardarEnLocalStorage();

  titulo.value = "";
  contenido.value = "";
}
</script>

<template>
  <main class="contenedor">
    <h1>Notas Rápidas</h1>

    <section class="formulario-nota">
      <input v-model="titulo" type="text" placeholder="Título de la nota" />

      <textarea
        v-model="contenido"
        placeholder="Escribe tu nota aquí..."
      ></textarea>

      <button @click="guardarNota">Guardar nota</button>
    </section>

    <section class="lista-notas">
      <h2>Mis notas</h2>

      <p v-if="notas.length === 0">No hay notas guardadas todavía.</p>

      <div v-for="nota in notas" :key="nota.id" class="nota">
        <h3>{{ nota.titulo }}</h3>
        <p>{{ nota.contenido }}</p>
      </div>
    </section>
  </main>
</template>

<style scoped>
.contenedor {
  max-width: 800px;
  margin: 40px auto;
  padding: 20px;
}

h1 {
  text-align: center;
}

.formulario-nota {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

input,
textarea {
  padding: 12px;
  font-size: 16px;
}

textarea {
  min-height: 120px;
}

button {
  padding: 12px;
  cursor: pointer;
}

.lista-notas {
  margin-top: 30px;
}

.nota {
  background-color: white;
  border: 1px solid #ddd;
  padding: 15px;
  margin-top: 15px;
  border-radius: 8px;
}
</style>
