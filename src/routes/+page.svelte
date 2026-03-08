<script lang="ts">
	import { onMount } from 'svelte';
	let clickedYes = false;

	let yesSize = 100;
	const maxYesSize = 384;

	let noTop = 0;
	let noLeft = 0;
	let noAbsolute = false;

	let showNoGif = false;

	let showEmojis = false;
	const emojis = ['🎉', '🥳', '💖', '✨', '💐', '🎆', '🍾', '😍'];
	let yesSound: HTMLAudioElement;

	onMount(() => {
		yesSound = new Audio('/sounds/celebration.mp3');
	});

	function acceptedInvite() {
		clickedYes = true;
		showEmojis = true;
		showNoGif = false;

		yesSound.currentTime = 0;
		yesSound.play();
	}

	function declinedInvitation() {
		showNoGif = true;

		if (yesSize < maxYesSize) {
			yesSize += 16;
		} else {
			noAbsolute = true;
			noTop = Math.floor(Math.random() * 80);
			noLeft = Math.floor(Math.random() * 80);
		}
	}
</script>

<div
	class="flex min-h-screen w-full flex-col justify-center items-center px-6 py-12 bg-pink-200 relative"
>
	<div class="mb-6 text-center">
		<p class="text-2xl glitter-text">Would you like to go on a date for our Anniversary?</p>
	</div>

	<div class="grid grid-cols-2 gap-6 w-full max-w-md relative">
		<button
			on:click={acceptedInvite}
			class="bg-green-500 text-white rounded-full text-lg font-semibold transition-all duration-300 cursor-pointer disabled:opacity-50 justify-self-center"
			style="width: {yesSize}px; height: {yesSize}px;"
			disabled={clickedYes}
		>
			Yes
		</button>

		<button
			on:click={declinedInvitation}
			class="bg-red-500 text-white rounded-full px-4 py-2 text-lg font-semibold transition-all duration-300 cursor-pointer disabled:opacity-50"
			style={noAbsolute ? `position:absolute; top:${noTop}%; left:${noLeft}%` : ''}
			disabled={clickedYes}
		>
			No
		</button>
	</div>

	<!-- Floating emojis on Yes click -->
	{#if showEmojis}
		{#each emojis as emoji}
			<div
				class="absolute text-3xl animate-float"
				style="top:{Math.random() * 80}%; left:{Math.random() * 80}%;"
			>
				{emoji}
			</div>
		{/each}
	{/if}

	<!-- Show GIF Condition -->
	{#if showNoGif}
		<img
			src="https://tenor.com/view/sus-dog-gif-27515537.gif"
			alt="No Clicked GIF"
			class="mt-6 w-96 h-96"
		/>
	{:else if clickedYes}
		<img
			src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExbjY1OWFwZHMzcTA0NHN1dzdxdXMwYXM1ejhhcjZveG80NGdjYWlqcCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/SzvV0A3n7gUCs/giphy.gif"
			alt="No Clicked GIF"
			class="mt-6 w-96 h-96"
		/>
	{/if}
</div>

<style>
	/* Floating animation for emojis */
	@keyframes float {
		0% {
			transform: translateY(0);
			opacity: 1;
		}
		50% {
			transform: translateY(-20px);
			opacity: 0.8;
		}
		100% {
			transform: translateY(0);
			opacity: 1;
		}
	}
	.animate-float {
		animation: float 1.5s ease-in-out infinite;
	}

	.glitter-text {
		font-family: 'Brush Script MT', cursive;
		font-size: 2rem;
		position: relative;
		background: linear-gradient(90deg, #0d0d0d, #2c2c54, #7f7fff, #f5f5f5);
		background-size: 300% 300%;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		animation: glitter 3s linear infinite;
		text-shadow:
			0 0 5px rgba(255, 255, 255, 0.2),
			0 0 10px rgba(255, 255, 255, 0.1);
	}

	@keyframes glitter {
		0% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0% 50%;
		}
	}
</style>
