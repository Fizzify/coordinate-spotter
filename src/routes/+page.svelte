<script lang="ts">
	let horizontalAxis = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'];
	let verticalAxis = ['1', '2', '3', '4', '5', '6', '7', '8'];
	let board: string[][] = [];

	let randomHorizontalAxisNumber = Math.floor(Math.random() * horizontalAxis.length);
	let randomVerticalAxisNumber = Math.floor(Math.random() * verticalAxis.length);

	let randomSquare = `${horizontalAxis[randomHorizontalAxisNumber]}${verticalAxis[randomVerticalAxisNumber]}`;
	let clickedSquares: string[] = [];
	let gameStarted: boolean = false;

	let score = 0;
	let elapsedTime = 0;

	for (let j = verticalAxis.length - 1; j >= 0; j--) {
		for (let i = 0; i < horizontalAxis.length; i++) {
			const number = j + i + 2;

			if (number % 2 === 0) {
				board = [...board, [horizontalAxis[i], verticalAxis[j], 'black-square']];
			} else {
				board = [...board, [horizontalAxis[i], verticalAxis[j], 'white-square']];
			}
		}
	}

	const handleSquareClick = (e: MouseEvent): void => {
		if (!gameStarted) {
			gameStarted = true;
			setInterval(() => {
				elapsedTime += 1;
			}, 1000);
			return;
		}

		const squareClicked = e.target as HTMLButtonElement;

		if (randomSquare === squareClicked.id) {
			squareClicked.style.backgroundColor = '#92B166';
			score += 1;
		} else {
			squareClicked.style.backgroundColor = '#C43D41';
		}

		clickedSquares = [...clickedSquares, squareClicked.id];
		setupNewRandomSquare();
	};

	const setupNewRandomSquare: any = () => {
		randomHorizontalAxisNumber = Math.floor(Math.random() * horizontalAxis.length);
		randomVerticalAxisNumber = Math.floor(Math.random() * verticalAxis.length);
		let newRandomSquare = `${horizontalAxis[randomHorizontalAxisNumber]}${verticalAxis[randomVerticalAxisNumber]}`;
		if (clickedSquares.includes(newRandomSquare)) return setupNewRandomSquare();
		if (clickedSquares.length === horizontalAxis.length * verticalAxis.length) return stopGame();
		randomSquare = newRandomSquare;
	};

	const stopGame = () => {};
</script>

<main>
	<header>
		<h1>{gameStarted ? randomSquare : 'Click any square to start!'}</h1>
		<p>Score: {score}</p>
		<p>Time: {elapsedTime}</p>
	</header>
	<div class="board">
		{#each board as square}
			<button on:click={handleSquareClick} class="square {square[2]}" id="{square[0]}{square[1]}" />
		{/each}
	</div>
	<footer><p>Made by Fizzify</p></footer>
</main>

<style>
	main {
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	header {
		text-align: center;
		margin: 1em 0;
	}

	footer {
		text-align: center;
		margin: 1em 0;
	}

	.board {
		display: flex;
		flex-wrap: wrap;
		width: 800px;
		height: 800px;
	}

	.square {
		width: 100px;
		height: 100px;
		border: none;
		outline: none;
	}

	.white-square {
		background-color: #dee3e6;
	}

	.black-square {
		background-color: #8ca2ad;
	}
</style>
