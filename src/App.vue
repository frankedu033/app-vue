<script setup lang="ts">
import { ref } from 'vue';

// 1. CONTRATO DE TIPADO (TypeScript Estricto)
interface Producto {
  id: number;
  nombre: string;
  precio: number;
  stock: number;
  imagen: string;
}

// 2. ESTADO REACTIVO
const isAuthenticated = ref<boolean>(false);
const username = ref<string>('');
const password = ref<string>('');
const errorMessage = ref<string>('');

// 3. DATOS DEL ALMACÉN
const listaProductos = ref<Producto[]>([
  {
    id: 101,
    nombre: "Teclado Mecánico RGB",
    precio: 250,
    stock: 12,
    imagen: "https://images.unsplash.com/photo-1511467687858-23d96c32e4ae?auto=format&fit=crop&q=80&w=400"
  },
  {
    id: 102,
    nombre: "Mouse Óptico Inalámbrico",
    precio: 110,
    stock: 20,
    imagen: "https://m.media-amazon.com/images/I/51F1X94Z--L._AC_UF894,1000_QL80_.jpg?auto=format&fit=crop&q=80&w=400"
  },
  {
    id: 103,
    nombre: "Monitor Gamer 24' 144Hz",
    precio: 1450,
    stock: 5,
    imagen: "https://images.unsplash.com/photo-1527443224154-c4a3942d3acf?auto=format&fit=crop&q=80&w=400"
  }
]);

// 4. LÓGICA DE CONTROL DE ACCESO
const handleLogin = (): void => {
  if (username.value === 'franklin.ecm' && password.value === 'itpm2026') {
    isAuthenticated.value = true;
    errorMessage.value = '';
  } else {
    errorMessage.value = '❌ Credenciales incorrectas. Intente de nuevo, colega.';
  }
};

const handleLogout = (): void => {
  isAuthenticated.value = false;
  username.value = '';
  password.value = '';
};

// 5. LÓGICA DE GESTIÓN DE STOCK
const incrementarStock = (id: number): void => {
  const producto = listaProductos.value.find(p => p.id === id);
  if (producto) {
    producto.stock += 1;
  }
};

const decrementarStock = (id: number): void => {
  const producto = listaProductos.value.find(p => p.id === id);
  if (producto && producto.stock > 0) {
    producto.stock -= 1;
  }
};
</script>

<template>
  <div class="container mt-5" style="font-family: 'Segoe UI', sans-serif; max-width: 1100px;">
    <!-- LOGIN -->
    <div v-if="!isAuthenticated" class="login-box">
      <div class="login-header">
        <h4 class="text-purple fw-bold mb-1">
          <i class="bi bi-lock-fill"></i> Sistema de Gestión - ITPM
        </h4>
        <small class="text-muted">Asignatura: DPW-207</small>
      </div>

      <form @submit.prevent="handleLogin" class="login-form">
        <div class="form-group">
          <label class="form-label fw-bold">Usuario Docente</label>
          <input
            v-model="username"
            type="text"
            class="form-control bg-dark text-white border-purple"
            placeholder="Ej: franklin.ecm"
            required
          />
        </div>

        <div class="form-group">
          <label class="form-label fw-bold">Contraseña</label>
          <input
            v-model="password"
            type="password"
            class="form-control bg-dark text-white border-purple"
            placeholder="••••••••"
            required
          />
        </div>

        <div v-if="errorMessage" class="alert alert-danger py-2 text-center small mb-3">
          {{ errorMessage }}
        </div>

        <button type="submit" class="btn btn-purple w-100 fw-bold py-2 shadow-sm">
          Ingresar al Sistema
        </button>
      </form>
    </div>

    <!-- PANEL DE ALMACÉN -->
    <div v-else class="panel-almacen">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h5>Panel de Almacén - Activo</h5>
        <button @click="handleLogout" class="btn-salir">Salir</button>
      </div>

      <h4>¡Bienvenido, Lic. Franklin Eduardo Condori Miranda!</h4>
      <p>Control de Inventarios en Tiempo Real</p>
      <hr />

      <!-- Catálogo Visual con Tarjetas (horizontal) -->
      <div class="row">
        <div v-for="p in listaProductos" :key="p.id" class="col">
          <div class="card h-100 shadow-sm border-0 bg-dark">
            <img :src="p.imagen" class="card-img-top" :alt="p.nombre"
                 style="height: 160px; object-fit: cover;" />

            <div class="card-body p-3 text-center">
              <h6 class="card-title fw-bold mb-1 text-purple">{{ p.nombre }}</h6>
              <p class="card-text text-muted small mb-2">ID: {{ p.id }}</p>

              <div class="d-flex justify-content-between align-items-center mb-2">
                <span class="badge bg-purple">Bs. {{ p.precio }}</span>
                <span class="text-light small fw-bold">{{ p.stock }} pzas.</span>
              </div>
            </div>

            <!-- Botón principal arriba y controles debajo -->
            <div class="card-footer bg-transparent border-0 p-2 text-center">
              <button class="btn btn-purple btn-sm w-100 fw-bold mb-2">
                Gestionar Stock
              </button>
              <div class="d-flex justify-content-center gap-2">
                <button
                  class="btn btn-outline-light btn-sm fw-bold"
                  @click="decrementarStock(p.id)"
                  :disabled="p.stock === 0"
                >
                  -
                </button>
                <button
                  class="btn btn-purple btn-sm fw-bold"
                  @click="incrementarStock(p.id)"
                >
                  +
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>