<script lang="ts">
	import cute from '$lib/assets/cute.gif';
	import kiss from '$lib/assets/kiss.gif';
	import { Confetti } from 'svelte-confetti';

	const getRandom = (limit: number): number => Math.random() * limit;
	const excuses: string[] = [
		'No',
		'Sorry',
		"I can't",
		'Maybe next time',
		'I have a boyfriend',
		'I have a girlfriend',
		'I have a partner',
		'I just want to be friends',
		"I'm not ready for a relationship"
	];

	let yesButton: HTMLButtonElement;
	let noButton: HTMLButtonElement;

	$: accepted = false;
	$: randomizePosition = false;
	$: initialButtonSize = 2.25;
	$: initialFontSize = 1.5;
	$: randomPosition = `translate(${getRandom(100)}%, ${getRandom(100)}%)`;
	$: randomExcuse = excuses[Math.floor(getRandom(excuses.length))];
</script>

<svelte:head>
	<title>❤️</title>
</svelte:head>

<div class="flex h-screen flex-col items-center justify-center gap-4">
	{#if accepted}
		<Confetti />
	{/if}
	<img src={accepted ? kiss : cute} alt={accepted ? 'Kiss gif' : 'Cute gif'} class="h-40 w-40" />
	<h1
		class="scroll-m-20 text-center font-cursive text-4xl font-extrabold tracking-tight text-primary lg:text-5xl"
	>
		{accepted ? 'Thank you & I love you <3' : 'Will you be my valentine?'}
	</h1>
	<div class="flex items-center justify-between gap-2" class:hidden={accepted}>
		<button
			class="inline-flex h-10 items-center justify-center whitespace-nowrap rounded-md bg-primary px-8 text-sm font-medium text-primary-foreground shadow transition-colors hover:bg-primary/90 focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50"
			bind:this={yesButton}
			on:click={() => (accepted = true)}>Yes</button
		>
		<button
			bind:this={noButton}
			style={`transform: ${randomizePosition ? randomPosition : 'translateX(0)'};`}
			class="inline-flex h-10 items-center justify-center whitespace-nowrap rounded-md bg-destructive px-8 py-2 text-sm font-medium text-destructive-foreground shadow-sm transition-colors hover:bg-destructive/90 focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50"
			on:click={() => {
				initialButtonSize += 2.25;
				initialFontSize += 0.5;
				randomPosition = `translate(${getRandom(100)}%, ${getRandom(100)}%)`;
				randomizePosition = true;
				yesButton.style.height = `${initialButtonSize}rem`;
				yesButton.style.width = `${initialButtonSize}rem`;
				yesButton.style.fontSize = `${initialFontSize}rem`;
				randomExcuse = excuses[Math.floor(getRandom(excuses.length))];
			}}>{randomExcuse}</button
		>
	</div>
</div>
