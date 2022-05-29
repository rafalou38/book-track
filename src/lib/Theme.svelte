<script>
	import { browser } from '$app/env';

	import { onMount } from 'svelte';

	let themeSet = false;
	let themeDark = false;

	onMount(() => {
		const stored = localStorage.getItem('theme');
		if (stored) {
			themeDark = stored === 'dark';
			themeSet = true;
		}
	});

	export function toggleDarkTheme() {
		themeDark = !themeDark;
		themeSet = true;
		localStorage.setItem('theme', themeDark ? 'dark' : 'light');
	}

	$: {
		if (browser && themeSet) {
			document.body.dataset.theme = themeDark ? 'dark' : 'light';
		}
	}
</script>

<div class="absolute right-0 top-0 p-1">
	<button class="btn btn-circle btn-ghost" on:click={toggleDarkTheme}>
		<svg
			xmlns="http://www.w3.org/2000/svg"
			xmlns:xlink="http://www.w3.org/1999/xlink"
			aria-hidden="true"
			role="img"
			class="iconify iconify--mdi"
			width="26"
			height="26"
			preserveAspectRatio="xMidYMid meet"
			viewBox="0 0 24 24"
			><path
				fill="currentColor"
				d="M7.5 2c-1.79 1.15-3 3.18-3 5.5s1.21 4.35 3.03 5.5C4.46 13 2 10.54 2 7.5A5.5 5.5 0 0 1 7.5 2m11.57 1.5l1.43 1.43L4.93 20.5L3.5 19.07L19.07 3.5m-6.18 2.43L11.41 5L9.97 6l.42-1.7L9 3.24l1.75-.12l.58-1.65L12 3.1l1.73.03l-1.35 1.13l.51 1.67m-3.3 3.61l-1.16-.73l-1.12.78l.34-1.32l-1.09-.83l1.36-.09l.45-1.29l.51 1.27l1.36.03l-1.05.87l.4 1.31M19 13.5a5.5 5.5 0 0 1-5.5 5.5c-1.22 0-2.35-.4-3.26-1.07l7.69-7.69c.67.91 1.07 2.04 1.07 3.26m-4.4 6.58l2.77-1.15l-.24 3.35l-2.53-2.2m4.33-2.7l1.15-2.77l2.2 2.54l-3.35.23m1.15-4.96l-1.14-2.78l3.34.24l-2.2 2.54M9.63 18.93l2.77 1.15l-2.53 2.19l-.24-3.34Z"
			/></svg
		>
	</button>
	<!-- <button class="btn btn-circle btn-ghost">
		<svg
			xmlns="http://www.w3.org/2000/svg"
			xmlns:xlink="http://www.w3.org/1999/xlink"
			aria-hidden="true"
			role="img"
			class="iconify iconify--material-symbols"
			width="26"
			height="26"
			preserveAspectRatio="xMidYMid meet"
			viewBox="0 0 24 24"
			><path
				fill="currentColor"
				d="M12 22q-2.05 0-3.875-.788q-1.825-.787-3.187-2.15q-1.363-1.362-2.15-3.187Q2 14.05 2 12q0-2.075.812-3.9q.813-1.825 2.201-3.175Q6.4 3.575 8.25 2.787Q10.1 2 12.2 2q2 0 3.775.688q1.775.687 3.112 1.9q1.338 1.212 2.125 2.875Q22 9.125 22 11.05q0 2.875-1.75 4.412Q18.5 17 16 17h-1.85q-.225 0-.312.125q-.088.125-.088.275q0 .3.375.862q.375.563.375 1.288q0 1.25-.688 1.85q-.687.6-1.812.6Zm-5.5-9q.65 0 1.075-.425Q8 12.15 8 11.5q0-.65-.425-1.075Q7.15 10 6.5 10q-.65 0-1.075.425Q5 10.85 5 11.5q0 .65.425 1.075Q5.85 13 6.5 13Zm3-4q.65 0 1.075-.425Q11 8.15 11 7.5q0-.65-.425-1.075Q10.15 6 9.5 6q-.65 0-1.075.425Q8 6.85 8 7.5q0 .65.425 1.075Q8.85 9 9.5 9Zm5 0q.65 0 1.075-.425Q16 8.15 16 7.5q0-.65-.425-1.075Q15.15 6 14.5 6q-.65 0-1.075.425Q13 6.85 13 7.5q0 .65.425 1.075Q13.85 9 14.5 9Zm3 4q.65 0 1.075-.425Q19 12.15 19 11.5q0-.65-.425-1.075Q18.15 10 17.5 10q-.65 0-1.075.425Q16 10.85 16 11.5q0 .65.425 1.075Q16.85 13 17.5 13Z"
			/></svg
		>
	</button> -->
</div>
