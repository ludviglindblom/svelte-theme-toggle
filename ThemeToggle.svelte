<script>
	import { onMount } from 'svelte';

	const ThemeLight =
		'<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><circle cx="12" cy="12" r="5" fill="currentColor"/><path fill="currentColor" d="M21 13h-1a1 1 0 0 1 0-2h1a1 1 0 0 1 0 2ZM4 13H3a1 1 0 0 1 0-2h1a1 1 0 0 1 0 2Zm13.66-5.66a1 1 0 0 1-.66-.29 1 1 0 0 1 0-1.41l.71-.71a1 1 0 1 1 1.41 1.41l-.71.71a1 1 0 0 1-.75.29ZM5.64 19.36a1 1 0 0 1-.71-.29 1 1 0 0 1 0-1.41l.71-.66a1 1 0 0 1 1.41 1.41l-.71.71a1 1 0 0 1-.7.24ZM12 5a1 1 0 0 1-1-1V3a1 1 0 0 1 2 0v1a1 1 0 0 1-1 1Zm0 17a1 1 0 0 1-1-1v-1a1 1 0 0 1 2 0v1a1 1 0 0 1-1 1ZM6.34 7.34a1 1 0 0 1-.7-.29l-.71-.71a1 1 0 0 1 1.41-1.41l.71.71a1 1 0 0 1 0 1.41 1 1 0 0 1-.71.29Zm12.02 12.02a1 1 0 0 1-.7-.29l-.66-.71A1 1 0 0 1 18.36 17l.71.71a1 1 0 0 1 0 1.41 1 1 0 0 1-.71.24Z"/></svg>';

	const ThemeDark =
		'<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill="currentColor" d="M20.21 15.32A8.56 8.56 0 1 1 11.29 3.5a.5.5 0 0 1 .51.28.49.49 0 0 1-.09.57A6.46 6.46 0 0 0 9.8 9a6.57 6.57 0 0 0 9.71 5.72.52.52 0 0 1 .58.07.52.52 0 0 1 .12.53Z"/></svg>';

	const THEMES = {
		LIGHT: 'light',
		DARK: 'dark'
	};

	const LABELS = {
		LIGHT: 'Change to dark theme',
		DARK: 'Change to light theme'
	};

	let hidden = $state(true);
	let button = $state(null);
	let currentThemeSetting = $state(THEMES.LIGHT);

	function calculateInitialTheme() {
		const savedTheme = localStorage.getItem('theme');
		if (savedTheme) {
			return savedTheme;
		}
		return window.matchMedia('(prefers-color-scheme: light)').matches ? THEMES.LIGHT : THEMES.DARK;
	}

	function updateButtonLabel(isLight) {
		const label = isLight ? LABELS.LIGHT : LABELS.DARK;
		button?.setAttribute('aria-label', label);
	}

	function setTheme(theme) {
		document.documentElement.setAttribute('data-theme', theme);
		localStorage.setItem('theme', theme);
		updateButtonLabel(theme === THEMES.LIGHT);
		currentThemeSetting = theme;
	}

	function toggleTheme() {
		const newTheme = currentThemeSetting === THEMES.LIGHT ? THEMES.DARK : THEMES.LIGHT;
		setTheme(newTheme);
	}

	onMount(() => {
		hidden = false;
		button = document.querySelector('[data-theme-toggle]');
		currentThemeSetting = calculateInitialTheme();
		setTheme(currentThemeSetting);
	});
</script>

<button
	type="button"
	aria-label={LABELS.LIGHT}
	data-theme-toggle
	onclick={toggleTheme}
	hidden={hidden || null}
>
	{#if currentThemeSetting === THEMES.DARK}
		{@html ThemeLight}
	{:else}
		{@html ThemeDark}
	{/if}
</button>

<style lang="css">
	:global([data-theme='{THEMES.LIGHT}']) {
		--color-theme: #000;
	}
	:global([data-theme='{THEMES.DARK}']) {
		--color-theme: #fff;
	}

	:global(svg) {
		display: block;
		width: 100%;
		height: 100%;
	}

	[data-theme-toggle] {
		width: 2rem;
		height: 2rem;
		background: none;
		border: none;
		cursor: pointer;
		padding: 0;
		color: var(--color-theme);
		transition: transform 0.2s ease-in-out;

		@media (hover: hover) {
			&:hover,
			&:focus-visible {
				transform: rotate(90deg);
			}
		}
	}
</style>
