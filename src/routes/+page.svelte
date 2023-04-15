<script>
	import { onMount } from 'svelte';
	import Weather from './Weather.svelte';
	import Search from './Search.svelte';
	import { fade } from 'svelte/transition';

	let searchHeight;
	let searchInput;

	onMount(() => {
		const search = document.getElementById('search');
		const searchStyle = getComputedStyle(search);
		let marginTop = parseFloat(searchStyle.marginTop);
		let marginBottom = parseFloat(searchStyle.marginBottom);
		searchHeight = search.offsetHeight + marginTop + marginBottom;
	});

	const options = {
		method: 'GET',
		headers: {
			'X-RapidAPI-Key': 'cfb3a20ee1mshd88b05317517c19p1416e6jsn12433ecaa3a9',
			'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com'
		}
	};
	let weather;

	function fetchWeather() {
		fetch(`https://weatherapi-com.p.rapidapi.com/forecast.json?q=${searchInput}&days=3`, options)
			.then((response) => response.json())
			.then((response) => {
				const { location, current, forecast } = response;
				const { country, name, localtime } = location;
				const { condition, temp_c, feelslike_c, humidity, is_day, pressure_mb, wind_kph } = current;
				const { code, text, icon } = condition;
				const { forecastday } = forecast;
				const [today] = forecastday;
				weather = {
					country,
					name,
					localtime,
					temperature: temp_c,
					feelsLike: feelslike_c,
					humidity,
					isDay: is_day,
					pressure: pressure_mb,
					windSpeed: wind_kph,
					code,
					weatherStatus: text,
					icon,
					today
				};
				return weather;
			});
	}

	function handleSearch(e) {
		searchInput = e.detail.search;
		fetchWeather();
	}
</script>

<main class="app gap-2">
	{#key weather?.isDay}
		<div
			transition:fade
			id="background"
			class={weather?.isDay === undefined
				? 'bg-default'
				: weather?.isDay === 1
				? 'bg-day'
				: 'bg-night'}
		/>
	{/key}
	<Search on:search={handleSearch} />
	{#if weather}
		<Weather {weather} {searchHeight} />
	{/if}
</main>

<style lang="postcss">
	.app {
		@apply relative  flex flex-col justify-start items-center w-full min-h-screen;
	}

	#background {
		position: fixed;
		width: 100%;
		min-height: 100%;
		background-size: cover;
		top: 0;
		z-index: -1;
	}

	.bg-default {
		background-image: url(https://r4.wallpaperflare.com/wallpaper/132/401/75/painting-clouds-sky-landscape-wallpaper-57bfb2f40fd59f8acf4284a1e191c785.jpg);
	}

	.bg-day {
		background-image: url(https://r4.wallpaperflare.com/wallpaper/880/841/120/mountains-forest-artwork-firewatch-wallpaper-82f06d94b7ca2320baa2a4bd5d75257d.jpg);
	}

	.bg-night {
		background-image: url(https://r4.wallpaperflare.com/wallpaper/615/294/495/artwork-deer-antlers-forest-wallpaper-3990080df11a5d7b7687880f2011c67d.jpg);
	}

	.app::before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -0;
		opacity: 0.5;
		background-color: rgba(0, 0, 0, 0.6);
	}
</style>
