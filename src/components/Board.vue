<script setup>
	import { ref, computed } from 'vue';

	const player = ref('X');
	const board = ref([
		['', '', ''],
		['', '', ''],
		['', '', ''],
	]);

	const CalculateWinner = (board) => {
		const lines = [
			[0, 1, 2],
			[3, 4, 5],
			[6, 7, 8],
			[0, 3, 6],
			[1, 4, 7],
			[2, 5, 8],
			[0, 4, 8],
			[2, 4, 6],
		];

		for (let i = 0; i < lines.length; i++) {
			const [a, b, c] = lines[i];

			if (board[a] && board[a] === board[b] && board[a] === board[c]) {
				return board[a];
			}
		}

		return null;
	};

	const winner = computed(() => CalculateWinner(board.value.flat()));

	const MakeMove = (x, y) => {
		if (winner.value) return;

		if (board.value[x][y]) return;

		board.value[x][y] = player.value;

		player.value = player.value === 'X' ? 'O' : 'X';
	};

	const ResetGame = () => {
		board.value = [
			['', '', ''],
			['', '', ''],
			['', '', ''],
		];
		player.value = 'X';
	};
</script>

<template>
	<main>
		<h1>Tic Tac Toe</h1>

		<h3>Player {{ player }}'s turn</h3>

		<div>
			<div v-for="(row, x) in board" :key="x" class="row">
				<div
					v-for="(cell, y) in row"
					:key="y"
					@click="MakeMove(x, y)"
					class="box"
				>
					<span class="cell">{{
						cell === 'X' ? 'X' : cell === 'O' ? 'O' : ''
					}}</span>
				</div>
			</div>
		</div>

		<div class="result-box">
			<h2 v-if="winner">Player '{{ winner }}' wins!</h2>
			<button @click="ResetGame">Reset</button>
		</div>
	</main>
</template>

<style>
	main {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}
	.row {
		display: flex;
	}
	.box {
		width: 50px;
		height: 50px;
		background-color: white;
		border: 1px solid black;
		display: flex;
		justify-content: center;
		align-items: center;
		font-weight: bold;
	}
	.cell {
		color: #000;
	}
	.result-box {
		text-align: center;
	}
</style>
