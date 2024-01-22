<script setup lang="ts">
import { computed, ref } from "vue";
import Casilla from "./components/casilla.vue";

enum FIGURA {
  X = "❌",
  O = "⭕️",
}
const resultado = ref<string | null>(null);
const turno = ref(FIGURA.X);
const tablero = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const resetear = () => {
  tablero.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  resultado.value = null;
};

const verificarVertical = (columna: number, figura: string) => {
  for (let i = 0; i < tablero.value.length; i++) {
    if (tablero.value[i][columna] !== figura) {
      return false;
    }
  }
  return true;
};

const verificarHorizontalmente = (fila: number, figura: string) => {
  for (let i = 0; i < tablero.value.length; i++) {
    if (tablero.value[fila][i] !== figura) {
      return false;
    }
  }
  return true;
};

const verificarDiagonalmente = (figura: string) => {
  if (
    tablero.value[0][0] === figura &&
    tablero.value[2][2] === figura &&
    tablero.value[1][1] === figura
  ) {
    return true;
  }
  if (
    tablero.value[2][0] === figura &&
    tablero.value[0][2] === figura &&
    tablero.value[1][1] === figura
  ) {
    return true;
  }
  return false;
};

const tableroVacio = computed(() => {
  for (let fila of tablero.value) {
    for (let columna of fila) {
      if (columna !== "") {
        return false;
      }
    }
  }
  return true;
});

const tableroLleno = computed(() => {
  for (let fila of tablero.value) {
    for (let columna of fila) {
      if (columna === "") {
        return false;
      }
    }
  }
  return true;
});

const cambiarEstadoCasilla = (fila: number, columna: number) => {
  if (!tablero.value[fila][columna] && !resultado.value) {
    tablero.value[fila][columna] = turno.value;
    turno.value = FIGURA.X === turno.value ? FIGURA.O : FIGURA.X;

    //Si Gano
    if (
      verificarVertical(columna, tablero.value[fila][columna]) ||
      verificarHorizontalmente(fila, tablero.value[fila][columna]) ||
      verificarDiagonalmente(tablero.value[fila][columna])
    ) {
      resultado.value = `GANO ${tablero.value[fila][columna]}`;
      alert(resultado.value);
      resetear();
    } else if (tableroLleno.value) {
      resultado.value = "EMPATE";
      alert(resultado.value);
      resetear();
    }
  }
};

const cambiarTurno = (figura: FIGURA) => {
  if (tableroVacio.value) {
    turno.value = figura;
  }
};

const activarBoton = (figura: string) => {
  return figura === turno.value ? "btn-activo" : "";
};
</script>

<template>
  <div class="nav">
    <h1>Tres en raya</h1>
    <span>Resultado:</span>
    <div>
      <button @click="resetear()">Resetear</button>
    </div>
    <button :class="activarBoton(FIGURA.X)" @click="cambiarTurno(FIGURA.X)">
      {{ FIGURA.X }}
    </button>
    <button :class="activarBoton(FIGURA.O)" @click="cambiarTurno(FIGURA.O)">
      {{ FIGURA.O }}
    </button>
  </div>
  <div class="fila" v-for="(fila, i) in tablero">
    <Casilla
      class="casilla"
      v-for="(columna, j) in fila"
      :figura="columna"
      @click="cambiarEstadoCasilla(i, j)"
    />
  </div>
</template>

<style scoped>
h1 {
  color: white;
}
.fila button {
  width: 100px;
  height: 100px;
  font-size: 50px;
  background-color: transparent;
  border: solid 5px green;
}
.fila {
  display: flex;
  align-items: center;
  justify-content: center;
}
.nav {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin: 1rem;
}
.nav span {
  color: white;
}
</style>
