<script setup lang="ts">
import { ref } from 'vue';

// 1. Tipado estricto con TypeScript
interface Producto {
  id: number;
  nombre: string;
  precio: number;
  stock: number;
}

// 2. Estado reactivo
const isAuthenticated = ref<boolean>(false);
const username = ref<string>('');
const password = ref<string>('');
const errorMessage = ref<string>('');

// 3. Datos del almacén
const listaProductos: Producto[] = [
  { id: 101, nombre: "Teclado Mecánico RGB", precio: 250, stock: 12 },
  { id: 102, nombre: "Mouse Óptico Inalámbrico", precio: 110, stock: 20 },
  { id: 103, nombre: "Monitor Gamer 24' 144Hz", precio: 1450, stock: 5 }
];

// 4. Lógica de autenticación
const handleLogin = (): void => {
  if (username.value === 'felix.maldonado' && password.value === 'itpm2026') {
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
</script>

<template>
  <div class="container">
    <!-- LOGIN -->
    <div v-if="!isAuthenticated" class="login-box">
      <h4>Sistema de Gestión - ITPM</h4>
      <small>Asignatura: DPW-207</small>

      <form @submit.prevent="handleLogin">
        <label>Usuario Docente</label>
        <input
          v-model="username"
          type="text"
          placeholder="Ej: felix.maldonado"
          required
        />

        <label>Contraseña</label>
        <input
          v-model="password"
          type="password"
          placeholder="••••••••"
          required
        />

        <div v-if="errorMessage" class="alert">
          {{ errorMessage }}
        </div>

        <button type="submit">Ingresar al Sistema</button>
      </form>
    </div>

    <!-- PANEL DE ALMACÉN -->
    <div v-else class="panel-almacen">
      <h5>Panel de Almacén - Activo</h5>
      <button @click="handleLogout" class="btn-salir">Salir</button>

      <h4>¡Bienvenido, Lic. Félix Maldonado!</h4>
      <p>Control de Inventarios en Tiempo Real</p>
      <hr />

      <div class="inventario">
        <div v-for="p in listaProductos" :key="p.id" class="item">
          <div class="nombre">{{ p.nombre }}</div>
          <div class="detalles">Bs. {{ p.precio }} | Stock: {{ p.stock }} pzas.</div>
          <div class="id">ID: {{ p.id }}</div>
          <span class="stock">Stock: {{ p.stock }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
