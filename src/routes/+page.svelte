<script lang="ts">
	let a = $state(1);
	let b = $state(1);
	let answer = $state('');
	let isCorrect = $state<boolean>();
	let lifes = $state(10);
	let score = $state(0);
	let gameOver = $derived(lifes <= 0);

	const init = () => {
		answer = '';
		isCorrect = undefined;
		a = Math.floor(Math.random() * 10);
		b = Math.floor(Math.random() * 10);
	};

	init();

	const onAnswer = (e: Event) => {
		e.preventDefault();
		try {
			const sum = parseInt(answer);
			if (a + b === sum) {
				isCorrect = true;
				score += 1;
				init();
			} else {
				isCorrect = false;
				lifes--;
			}
		} catch (e) {
			isCorrect = false;
			lifes--;
		}
	};
	const reset = () => {
		lifes = 10;
		score = 0;
		init();
	};
</script>

<div class="max-w-lg m-auto">
	<h1 class="text-3xl font-bold p-4">Mathe</h1>

	<div class="p-4">
		{#if gameOver}
			<div class="text-xl">Game Over</div>
			<div>Du hast {score} Punkte erreicht.</div>
			<button class="px-4 py-2 cursor-pointer rounded bg-slate-400" onclick={reset}>nochmal?</button
			>
		{:else}
			<div class="flex justify-between">
				<div>
					{#each { length: lifes }}♥{/each}
				</div>
				<div>
					🟡 {score}
				</div>
			</div>
			<form onsubmit={onAnswer} class="flex justify-center">
				<div class="text-3xl mt-8">
					{a} + {b} =
					<input
						bind:value={answer}
						type="text"
						step={1}
						placeholder=""
						class="border-b w-9 text-right px-1"
					/>
				</div>
				{#if isCorrect === true}
					<div>Das ist richtig.</div>
				{/if}
				{#if isCorrect === false}
					<div>Das ist leider falsch.</div>
				{/if}
			</form>
		{/if}
	</div>
</div>
