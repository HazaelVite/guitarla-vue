<script setup>
import { onMounted, ref } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const carrito = ref([]);
const guitarraDestacada = ref([]);

onMounted(() => {
  guitarras.value = db;
  guitarraDestacada.value = db[3];
});

const agregarCarrito = (guitarra) => {
  const existe = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id
  );
  if (existe >= 0) {
    if (carrito.value[existe].cantidad >= 5) return;
    carrito.value[existe].cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }
};

const aumentarCantidad = (id) => {
  const existe = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[existe].cantidad >= 5) {
    return;
  }
  carrito.value[existe].cantidad++;
};

const disminuirCantidad = (id) => {
  const existe = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[existe].cantidad <= 1) {
    carrito.value.splice(existe, 1);
    return;
  }
  carrito.value[existe].cantidad--;
};

const vaciarCarrito = () => {
  carrito.value = [];
};

const eliminarProductoCarrito = (id) => {
  const existe = carrito.value.findIndex((producto) => producto.id === id);
  carrito.value.splice(existe, 1);
};
</script>

<template>
  <Header
    :carrito="carrito"
    :guitarra="guitarraDestacada"
    @agregar-carrito="agregarCarrito"
    @aumentar-cantidad="aumentarCantidad"
    @disminuir-cantidad="disminuirCantidad"
    @eliminar-producto-carrito="eliminarProductoCarrito"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      />
      <!-- FIN GUITARRA -->
    </div>
  </main>
  <Footer />
</template>
