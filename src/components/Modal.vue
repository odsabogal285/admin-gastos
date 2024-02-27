<script setup>
  import {ref, onMounted} from "vue";
  import cerrarModal from '../assets/img/cerrar.svg'
  import Alerta from "./Alerta.vue";

  const error = ref('');

  const emit = defineEmits(['ocultar-modal', 'guardar-gasto','update:nombre', 'update:cantidad', 'update:categoria'])
  const props = defineProps({
    modal: {
      type: Object,
      required: true
    },
    nombre: {
      type: String,
      required: true
    },
    cantidad: {
      type: [String, Number],
      required: true
    },
    categoria: {
      type: String,
      required: true
    },
    disponible: {
      type: Number,
      required: true
    }
  })

  const agregarGasto = () => {
    const { nombre, cantidad, categoria, disponible} = props;
    if ([nombre, cantidad, categoria].includes('')){
      error.value = 'Todos los campos son obligatorios';
      setTimeout(() => {
        error.value = ''
      }, 2000);
      return
    }

    if(cantidad <= 0) {
      error.value = 'Cantidad no valida'
      setTimeout(() => {
        error.value = ''
      }, 2000);
      return
    }

    if(cantidad > disponible) {
      error.value = 'Haz excedido el presupuesto'
      setTimeout(() => {
        error.value = ''
      }, 3000);
      return
    }

    emit('guardar-gasto');

  }
</script>

<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img
          :src="cerrarModal"
          @click="$emit('ocultar-modal')"
      />
    </div>
    <div class="contenedor contenedor-formulario"
      :class="[modal.animar ? 'animar':'cerrar']"
    >
      <form
          class="nuevo-gasto"
          @submit.prevent="agregarGasto"
      >
        <legend>Añadir gastos</legend>
        <div class="campo">
          <label for="nombre">Nombre gasto: </label>
          <Alerta v-if="error">{{error}}</Alerta>
          <input
              type="text"
              id="nombre"
              placeholder="Añade el Nombre del Gasto"
              :value="nombre"
              @input="$emit('update:nombre', $event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="cantidad">Cantidad: </label>
          <input
              type="text"
              id="cantidad"
              placeholder="Añade la cantidad del Gasto, ej. 300"
              :value="cantidad"
              @input="$emit('update:cantidad', +$event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="categoria">Categoría: </label>
          <select
            id="categoria"
            :value="categoria"
            @input="$emit('update:categoria', $event.target.value)"
          >
            <option value="">-- Selecione --</option>
            <option value="ahorro">Ahorro</option>
            <option value="comida">Comida</option>
            <option value="casa">Casa</option>
            <option value="gastos">Gastos Varios</option>
            <option value="ocio">Ocio</option>
            <option value="salud">Salud</option>
            <option value="suscripciones">Suscripciones</option>
          </select>
        </div>
        <input
          type="submit"
          value="Añadir gasto"
        />
      </form>
    </div>
  </div>
</template>

<style scoped>
  .modal {
    position: absolute;
    background-color: rgba(0 0 0 / 0.9);
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
  .contenedor-formulario {
    transition-property: all;
    transition-duration: 300ms;
    transition-timing-function: ease-in;
    opacity: 0;
  }
  .contenedor-formulario.animar {
    opacity: 1;
  }
  .contenedor-formulario.cerrar {
    opacity: 0;
  }

  .cerrar-modal {
    position: absolute;
    top: 3rem;
    right: 3rem;
  }
  .cerrar-modal img {
    width: 3rem;
    cursor: pointer;
  }
  .nuevo-gasto {
    margin: 10rem auto 0 auto;
    display: grid;
    gap: 2rem;
  }
  .nuevo-gasto legend {
    text-align: center;
    color: var(--blanco);
    font-size: 3rem;
    font-weight: 700;
  }
  .campo {
    display: grid;
    gap: 2rem;
  }
  .nuevo-gasto input,
  .nuevo-gasto select {
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
  }

  .nuevo-gasto label {
    color: var(--blanco);
    font-size: 3rem;
  }

  .nuevo-gasto input[type="submit"] {
    background-color: var(--azul);
    color: var(--blanco);
    font-weight: 700;
    cursor: pointer;
  }
</style>