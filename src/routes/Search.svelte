<script>
	import { createEventDispatcher } from 'svelte';
	let clicked = false;
	const dispatch = createEventDispatcher();
	let search = '';

	function handleInput(e) {
		if (e.key === 'Enter' && search !== '') {
			if (clicked) {
				onSearch();
			} else {
				clicked = true;
			}
		}
	}

	function onSearch() {
		dispatch('search', { search: search });
		search = '';
	}
</script>

<div
	on:transitionend={onSearch}
	id="search"
	class="input-position my-4"
	style={`top: ${clicked ? '0' : '45%'}`}
>
	<div class="input-holder">
		<span style="border-radius: 30%" class="material-icons absolute right-2"> search </span>
		<span class="material-icons text-[#EA4335] absolute left-2" left> place </span>
		<input
			bind:value={search}
			on:keydown={handleInput}
			type="text"
			placeholder="Enter your location"
			class="focus:outline-none font-semibold text-lg px-12 bg-transparent h-full w-full rounded-lg"
		/>
	</div>
</div>

<style lang="postcss">
	.input-position {
		@apply absolute w-2/4 h-20 bg-white rounded-xl;
		transition: all 500ms ease;
	}

	.input-holder {
		@apply relative bg-transparent flex items-center h-full w-full rounded-lg;
		transition: transform 350ms ease-out;
	}

	.material-icons {
		@apply text-4xl;
	}

	input,
	input::placeholder {
		@apply text-2xl;
	}

	input {
		@apply text-[#282828];
	}

	@media all and (max-width: 420px) {
		.input-position {
			@apply h-16 w-[85%];
		}

		input,
		input::placeholder {
			@apply text-base;
		}

		.material-icons {
			@apply text-3xl;
		}
	}
</style>
