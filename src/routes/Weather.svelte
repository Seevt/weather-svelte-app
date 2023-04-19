<script>
	import { fade } from 'svelte/transition';
	export let searchHeight;
	export let weather;
	let localTime = weather.localtime.split(' ');
	$: hourlyForecast = weather.today.hour;

	function getIcon(icon_url) {
		const regex = /\d+/g;
		let icon_code = icon_url.match(regex);
		return `/icons/${weather.isDay ? 'day' : 'night'}/${icon_code[2]}.svg`;
	}
	$: newStatus = weather.weatherStatus.split('').length ? '' : '';
</script>

<main
	transition:fade={{
		duration: 900,
	}}
	class="weather"
	style={`margin: ${searchHeight}px 0 0 0`}
>
	<div class="weather-container">
		<!-- buscar que no se renderizen ambos a la vez -->
		<div class="status-1">
			<p class="pl-1">
				{weather.name}, {weather.country}
				<span>
					As of {localTime[1]}

					{localTime[1] > '12' ? 'pm' : 'am'}
				</span>
			</p>
			<div class="temp-and-condition pl-1">
				<p>{Math.round(weather.temperature)}ºC</p>
				<p>{weather.weatherStatus}</p>
			</div>
		</div>

		<div class="status-2">
			<img class="climate-icon" src={getIcon(weather.icon)} alt="weather status icon" />
		</div>
	</div>
	<div class="more-info">
		<div class="category">
			<p>Feels like</p>
			<span>{Math.round(weather.feelsLike)}ºC</span>
		</div>
		<div class="category">
			<p>Wind</p>
			<span>{Math.round(weather.windSpeed)} km/h</span>
		</div>
		<div class="category">
			<p>Humidity</p>
			<span>{weather.humidity}%</span>
		</div>
		<div class="category">
			<p>Pressure</p>
			<span>{Math.round(weather.pressure)} hPa</span>
		</div>
	</div>

	<hr />
	<h3 id="hours-header" class="text-3xl font-bold">Hourly Forecast</h3>
	<div class="hours">
		<div class="time">
			<div class="forecast-temp-and-condition">
				<img src={getIcon(hourlyForecast[8].condition.icon)} alt="" />
				<p>{hourlyForecast[8].condition.text}</p>
			</div>
			<p>{hourlyForecast[8].time.split(' ')[1]}</p>
		</div>
		<div class="time">
			<div class="forecast-temp-and-condition">
				<img src={getIcon(hourlyForecast[12].condition.icon)} alt="" />
				<p>{hourlyForecast[12].condition.text}</p>
			</div>
			<p>{hourlyForecast[12].time.split(' ')[1]}</p>
		</div>
		<div class="time">
			<div class="forecast-temp-and-condition">
				<img src={getIcon(hourlyForecast[16].condition.icon)} alt="" />
				<p>{hourlyForecast[16].condition.text}</p>
			</div>
			<p>{hourlyForecast[16].time.split(' ')[1]}</p>
		</div>
		<div class="time">
			<div class="forecast-temp-and-condition">
				<img src={getIcon(hourlyForecast[20].condition.icon)} alt="" />
				<p>{hourlyForecast[20].condition.text}</p>
			</div>
			<p>{hourlyForecast[20].time.split(' ')[1]}</p>
		</div>
	</div>
</main>

<style lang="postcss">
	.weather {
		@apply flex flex-col gap-6 w-2/4  bg-transparent;
		min-height: 100%;
		backdrop-filter: blur(0px);
		color: white;
	}

	.weather-container {
		@apply flex  w-full font-bold text-xl;
	}

	.status-1 {
		@apply flex justify-evenly gap-2  w-1/2 flex-col;
		min-height: fit-content;
	}

	.status-1 p:first-of-type {
		@apply text-2xl;
	}

	.temp-and-condition {
		@apply flex flex-col gap-2;
	}

	.temp-and-condition p:first-of-type {
		@apply text-5xl font-extrabold;
	}

	.temp-and-condition p:last-of-type {
		@apply text-4xl font-semibold;
	}

	.status-2 {
		@apply flex items-end flex-col justify-center  w-1/2;
	}

	.more-info {
		@apply grid w-full rounded-xl p-4 drop-shadow-md;
		grid-template-columns: repeat(4, 1fr);
		grid-gap: 10px;
		background: rgba(196, 196, 196, 0.2);
		box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
	}

	.category {
		@apply flex flex-col items-center;
	}

	.category p {
		@apply text-lg  mb-1 font-semibold;
	}

	.category span {
		@apply tracking-wide text-center text-2xl font-extrabold;
	}

	.climate-icon {
		@apply h-[60%];
		object-fit: cover;
	}

	@media all and (max-width: 420px) {
		.weather {
			@apply w-[85%];
		}

		.more-info {
			grid-template-columns: repeat(2, 1fr);
			justify-content: space-between;
			@apply px-0;
		}

		.category span {
			@apply text-xl;
		}

		.status-1 {
			@apply w-4/6;
		}

		.status-1 p:first-of-type {
			@apply text-xl;
		}

		.status-1 p span {
			display: none;
		}

		.status-2 img {
			@apply h-16;
		}

		.temp-and-condition {
			@apply flex-col gap-0;
		}

		.temp-and-condition p:first-of-type {
			@apply font-extrabold text-3xl;
		}
		.temp-and-condition p:last-of-type {
			@apply h-fit;
			margin: 0;
			font-size: 1.5rem !important;
		}

		.climate-icon {
			@apply h-[25%];
		}
	}

	hr {
		@apply rounded-xl;
		border: 1px solid rgba(255, 255, 255, 0.5);
	}

	.hours {
		@apply grid w-full rounded-xl p-4 drop-shadow-md;
		grid-template-columns: repeat(4, 1fr);
		grid-gap: 10px;
	}

	.time {
		@apply flex flex-col gap-1  items-center;
	}

	.time p:last-of-type {
		@apply text-center text-xl font-bold;
	}

	.forecast-temp-and-condition {
		@apply flex gap-1  flex-col items-center justify-start;
	}

	.forecast-temp-and-condition img {
		@apply h-12;
	}
	.forecast-temp-and-condition p {
		font-size: 1.2rem !important;
	}

	@media all and (max-width: 420px) {
		.hours {
			display: flex;
			flex-direction: column;
			@apply gap-4 p-0;
		}

		.time {
			@apply flex-row  justify-between;
		}

		.forecast-temp-and-condition {
			@apply flex flex-row items-center gap-4 w-[70%];
		}

		.forecast-temp-and-condition img {
			@apply h-12;
		}

		.forecast-temp-and-condition p {
			font-size: 1rem !important;
		}

		.time p:last-of-type {
			@apply text-lg;
		}

		#hours-header {
			@apply text-xl;
		}
	}
</style>
