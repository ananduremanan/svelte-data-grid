<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let show = false;
	export let columnHeader = '';

	$: show;

	let filterValue: string = '';
	let selected: string = 'contains';
	let isFilterActive = false;

	const onCancel = () => {
		dispatch('cancel');
	};

	const applyFilter = () => {
		dispatch('apply', { filterValue, selected, columnHeader });
		show = false;
		isFilterActive = true;
	};

	const clearFilter = () => {
		isFilterActive = false;
		dispatch('clearFilter', { columnHeader });
	};
</script>

{#if show}
	<div class="absolute bg-gray-100 p-2 border z-10 mt-40 flex items-end flex-col shadow-lg gap-2">
		<select name="selected" id="selected" bind:value={selected} class="w-full p-2 rounded-lg">
			<option value="contains">Contains</option>
			<option value="starts_with">Starts With</option>
			<option value="ends_with">Ends With</option>
		</select>
		<input
			type="text"
			bind:value={filterValue}
			placeholder="Enter filter value"
			class="rounded-lg p-2 text-sm"
		/>
		<div class="mt-2">
			<button on:click={onCancel} class="text-xs bg-red-400 p-1 rounded-lg hover:bg-red-600"
				>Cancel</button
			>
			{#if isFilterActive}
				<button on:click={clearFilter} class="text-xs bg-blue-500 p-1 rounded-lg hover:bg-blue-600"
					>Clear Filter</button
				>
			{/if}
			<button on:click={applyFilter} class="text-xs bg-blue-500 p-1 rounded-lg hover:bg-blue-600"
				>Apply Filter</button
			>
		</div>
	</div>
{/if}
