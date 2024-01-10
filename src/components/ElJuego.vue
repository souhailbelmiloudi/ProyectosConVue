<script setup>
import { ref, computed } from 'vue'

const Jugador = ref("X")
const Tablero = ref([
    ["", "", ""],
    ["", "", ""],
    ["", "", ""]
])

const calcularGanador = (squares) => {
    const lineas = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], // Horizontales
        [0, 3, 6], [1, 4, 7], [2, 5, 8], // Verticales
        [0, 4, 8], [2, 4, 6] // Diagonales
    ]
    for (let i = 0; i < lineas.length; i++) {
        const [a, b, c] = lineas[i]
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a]
        }
    }
    return null
}

// Calcula el ganador el flat() es para convertir el array  en uno solo Nivel
const ganador = computed(() => calcularGanador(Tablero.value.flat()))

const click = (x, y) => {
    if (ganador.value) return
    if (Tablero.value[x][y] || ganador.value) return
    Tablero.value[x][y] = Jugador.value
    Jugador.value = Jugador.value === "X" ? "O" : "X"
}

const reset = () => {
    Jugador.value = "X"
    Tablero.value = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
    ]
}

const estiloDeCelda = (col) => {
    return [
        'w-20 h-20 border border-white flex items-center justify-center material-icons-outlined text-4xl cursor-pointer hover:bg-gray-700',
        col === 'X' ? 'text-red-500' : col === 'O' ? 'text-blue-500' : ''
    ]
}

const estiloDeGanador = () => {
    return [
        'text-6xl font-bold mb-8',
        ganador.value === 'X' ? 'text-red-500' : ganador.value === 'O' ? 'text-blue-500' : ''
    ]
}

</script>


<template>
    <main class="pt-8 text-center min-h-screen bg-gray-800 text-white">

        <h1 class="pt-8 text-4xl font-bold">Tres en Raya</h1>
        <h3 class="pt-8 text-2xl font-bold">Turno de {{ Jugador }}</h3>
        <div class="flex flex-col items-center mb-8">
            <div v-for="(row, x) in Tablero" :key="x" class="flex">
                <div v-for="(col, y) in row" :key="y" @click="click(x, y)" :class="estiloDeCelda(col)">
                    {{ col === 'X' ? 'close' : col === 'O' ? 'panorama_fish_eye' : '' }}
                </div>
            </div>

        </div>

        <div v-if="ganador" class="mb-8">
            <h2 :class="estiloDeGanador()">🥇El ganador es {{ ganador }} 🥇</h2>

        </div>
        <button @click="reset" class="px-4 py-2 
                  bg-pink-700 
                  hover:bg-pink-800
                  duration-300
                  rounded-md">
            Reiniciar
        </button>



    </main>
</template>





