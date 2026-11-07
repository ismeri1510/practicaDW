<script setup>
import { ref, onMounted } from "vue";

const titulo = ref("");
const contenido = ref("");
const notas = ref([]);

const editando = ref(false);
const idEditando = ref(null);

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

  if (editando.value) {
    const nota = notas.value.find((nota) => nota.id === idEditando.value);

    if (nota) {
      nota.titulo = titulo.value;
      nota.contenido = contenido.value;
    }

    editando.value = false;
    idEditando.value = null;
  } else {
    const nuevaNota = {
      id: Date.now(),
      titulo: titulo.value,
      contenido: contenido.value,
    };

    notas.value.push(nuevaNota);
  }

  guardarEnLocalStorage();

  titulo.value = "";
  contenido.value = "";
}

function editarNota(nota) {
  titulo.value = nota.titulo;
  contenido.value = nota.contenido;

  editando.value = true;
  idEditando.value = nota.id;
}

function eliminarNota(id) {
  const confirmar = confirm("¿Deseas eliminar esta nota?");

  if (!confirmar) {
    return;
  }

  notas.value = notas.value.filter((nota) => nota.id !== id);

  guardarEnLocalStorage();

  if (idEditando.value === id) {
    titulo.value = "";
    contenido.value = "";
    editando.value = false;
    idEditando.value = null;
  }
}

function cancelarEdicion() {
  titulo.value = "";
  contenido.value = "";
  editando.value = false;
  idEditando.value = null;
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

      <button @click="guardarNota">
        {{ editando ? "Actualizar nota" : "Guardar nota" }}
      </button>

      <button v-if="editando" class="btn-cancelar" @click="cancelarEdicion">
        Cancelar edición
      </button>
    </section>

    <section class="lista-notas">
      <h2>Mis notas</h2>

      <p v-if="notas.length === 0">No hay notas guardadas todavía.</p>

      <div v-for="nota in notas" :key="nota.id" class="nota">
        <h3>{{ nota.titulo }}</h3>

        <p>{{ nota.contenido }}</p>

        <div class="acciones">
          <button @click="editarNota(nota)">Editar</button>

          <button class="btn-eliminar" @click="eliminarNota(nota.id)">
            Eliminar
          </button>
        </div>
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
  border: 1px solid #ccc;
  border-radius: 6px;
}

textarea {
  min-height: 120px;
  resize: vertical;
}

button {
  padding: 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  background-color: #2563eb;
  color: white;
}

button:hover {
  opacity: 0.9;
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

.acciones {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.btn-eliminar {
  background-color: #dc2626;
}

.btn-cancelar {
  background-color: #6b7280;
}
</style>
