<script setup>
import { onMounted, ref } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";

const guitarras = ref([]);
const carrito = ref([]);

onMounted(() => {
  guitarras.value = db;
});

const agregarCarrito = (guitarra) => {
  const existe = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id
  );
  if (existe >= 0) {
    carrito.value[existe].cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }
};
</script>

<template>
  <Header :carrito="carrito" />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        :key="index"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      />
      <!-- FIN GUITARRA -->
    </div>
  </main>
  <Footer />
</template>
