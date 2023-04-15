<script>
	export let searchHeight;
	export let weather;
	let localTime = weather.localtime.split(' ');
	$: hourlyForecast = weather.today.hour;

	function getIcon(icon_url) {
		const regex = /\d+/g;
		let icon_code = icon_url.match(regex);
		return `/icons/${weather.isDay ? 'day' : 'night'}/${icon_code[2]}.svg`;
	}
</script>

<main class="weather" style={`margin: ${searchHeight}px 0 0 0`}>
	<div class="weather-container">
		{#key weather?.name}
			<!-- buscar que no se renderizen ambos a la vez -->
			<div class="status-1">
				<p class="pl-1">
					{weather.name}, {weather.country} As of {localTime[1]}
					{localTime[1] > '12' ? 'pm' : 'am'}
				</p>
				<p class="text-8xl font-extrabold">{Math.round(weather.temperature)}ºC</p>
				<p class="pl-1">{weather.weatherStatus}</p>
			</div>
		{/key}

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
	<h3 class="text-3xl font-bold">Hourly Forecast</h3>
	<div class="hours">
		<div class="time">
			<p>{hourlyForecast[8].time.split(' ')[1]}</p>
			<img src={getIcon(hourlyForecast[8].condition.icon)} alt="" />
			<p>{hourlyForecast[8].condition.text}</p>
		</div>
		<div class="time">
			<p>{hourlyForecast[12].time.split(' ')[1]}</p>
			<img src={getIcon(hourlyForecast[12].condition.icon)} alt="" />
			<p>{hourlyForecast[12].condition.text}</p>
		</div>
		<div class="time">
			<p>{hourlyForecast[16].time.split(' ')[1]}</p>
			<img src={getIcon(hourlyForecast[16].condition.icon)} alt="" />
			<p>{hourlyForecast[16].condition.text}</p>
		</div>
		<div class="time">
			<p>{hourlyForecast[20].time.split(' ')[1]}</p>
			<img src={getIcon(hourlyForecast[20].condition.icon)} alt="" />
			<p>{hourlyForecast[20].condition.text}</p>
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

	.status-1 p:last-of-type {
		@apply text-4xl;
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
		@apply text-base  mb-1 font-semibold;
	}

	.category span {
		@apply tracking-wide text-center text-2xl font-extrabold;
	}

	.climate-icon {
		@apply h-[60%];
		object-fit: cover;
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
		@apply flex flex-col  items-center;
	}

	.time p:first-of-type {
		@apply text-lg font-bold;
	}

	.time p:last-of-type {
		@apply text-center text-base font-bold;
	}

	.time img {
		@apply w-10 h-10;
	}
</style>
