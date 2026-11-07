<script setup></script>

<template>
  <main class="contenedor">
    <h1>Notas Rápidas</h1>

    <section class="formulario-nota">
      <input type="text" placeholder="Título de la nota" />

      <textarea placeholder="Escribe tu nota aquí..."></textarea>

      <button>Guardar nota</button>
    </section>

    <section class="lista-notas">
      <h2>Mis notas</h2>

      <p>No hay notas guardadas todavía.</p>
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
</style>
