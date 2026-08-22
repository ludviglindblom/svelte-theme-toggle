<script>
	import { onMount } from 'svelte';
  import ThemeLight from './ThemeLight.svelte';
  import ThemeDark from './ThemeDark.svelte';

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
		<ThemeLight />
	{:else}
    <ThemeDark />
	{/if}
</button>

<style lang="css">
	:global([data-theme='light']) {
		--color-theme: #000;
	}
	:global([data-theme='dark']) {
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
