<script>
	import { createEventDispatcher } from 'svelte';
	let clicked = false;
	const dispatch = createEventDispatcher();
	let search = '';

	function handleInput(e) {
		if (e.keyCode === 13 && search !== '') {
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
		transition: all 350ms ease-out;
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
</style>
