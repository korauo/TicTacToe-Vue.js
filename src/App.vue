<script setup>
    import { ref, computed } from 'vue'

    const player = ref('X')
    const board = ref([
    ['', '', ''],
    ['', '', ''],
    ['', '', '']
    ])

    const CalculateWinner = (board) => {
    const lines = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]]

    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i]

        if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        return board[a]
        }
    }

    return null
    }

    const winner = computed(() => CalculateWinner(board.value.flat()))

    const MakeMove = (x, y) => {
        if (winner.value) return

        if (board.value[x][y]) return

        board.value[x][y] = player.value

        player.value = player.value === 'X' ? 'O' : 'X'
    }

    const ResetGame = () => {
        board.value = [
            ['', '', ''],
            ['', '', ''],
            ['', '', '']
        ]
        player.value = 'X'
    }

</script>

<template>
  <div class="loader-wrapper" v-if="showLoader">
    <div class="loader"><div class="loader-inner"></div></div>
  </div>

	<main class="pt-8 text-center antialiased">
		<h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>

		<h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>

		<div class="flex flex-col items-center mb-8">
			<div 
				v-for="(row, x) in board" 
				:key="x"
				class="flex">
				<div 
					v-for="(cell, y) in row" 
					:key="y" 
					@click="MakeMove(x, y)" 
					:class="`border-2 border-gray-900 w-24 h-24 hover:bg-gray-700 duration-300 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${cell === 'X' ? 'text-pink-500' : 'text-blue-400'}`">
					{{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
				</div>
			</div>
		</div>

		<div class="text-center">
			<h2 v-if="winner" class="text-3xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
			<button @click="ResetGame" class="px-4 py-2 bg-pink-500 rounded-xl hover:rounded-md uppercase font-bold hover:bg-pink-600 duration-700">Reset</button>
		</div>
	</main>
</template>

<script>
    export default {
        data() {
            return {
                showLoader: true
            }
        },

        created() {
            setTimeout(() => this.showLoader = false, 2000)
        }
    }
</script>


<style>
    body {
        @apply bg-gray-800 text-white;
    }
    .loader-wrapper {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        background-color: #242f3f;
        display:flex;
        justify-content: center;
        align-items: center;
    }
    .loader {
        display: inline-block;
        width: 30px;
        height: 30px;
        position: relative;
        border: 4px solid #Fff;
        animation: loader 2s infinite ease;
    }
    .loader-inner {
        vertical-align: top;
        display: inline-block;
        width: 100%;
        background-color: #fff;
        animation: loader-inner 2s infinite ease-in;
    }
    @keyframes loader {
        0% { transform: rotate(0deg);}
        25% { transform: rotate(180deg);}
        50% { transform: rotate(180deg);}
        75% { transform: rotate(360deg);}
        100% { transform: rotate(360deg);}
    }
    @keyframes loader-inner {
        0% { height: 0%;}
        25% { height: 0%;}
        50% { height: 100%;}
        75% { height: 100%;}
        100% { height: 0%;}
    }
</style>
