<script lang="ts">
	import { onMount } from 'svelte';

	import Shape from '../components/shape.svelte';

	const shapeOptions = ['circle', 'square'];

	let configs: Config[] = [];
	let backgroundColor = 'grey';
	let shape = shapeOptions[0];
	let shapeColor = 'red';
	let isCircle = true;
	let configName = 'Default';

	$: {
		if (shape === 'circle') {
			isCircle = true;
		} else {
			isCircle = false;
		}
	}

	function saveConfig() {
		const current = {
			name: configName,
			backgroundColor,
			shape,
			shapeColor
		};

		configs = [...configs, current];
		localStorage.setItem('configs', JSON.stringify(configs));
	}

	interface Config {
		name: string;
		backgroundColor: string;
		shape: string;
		shapeColor: string;
	}

	function loadConfig(config: Config) {
		backgroundColor = config.backgroundColor;
		shape = config.shape;
		shapeColor = config.shapeColor;
		configName = config.name;
	}

	onMount(() => {
		const cachedConfig = localStorage.getItem('configs') || '[]';

		configs = JSON.parse(cachedConfig);
	});
</script>

<template>
	<div style="background-color: {backgroundColor};" class="min-h-screen flex flex-col">
		<div class="flex flex-col gap-2 w-1/2 mx-auto pb-4">
			<Shape {isCircle} color={shapeColor} additionalClass="mx-auto" />

			<div class="flex flex-col bg-white p-4 rounded-md shadow-lg">
				<span>Background Color</span>
				<input bind:value={backgroundColor} />
				<span>Trigger Color</span>
				<input bind:value={shapeColor} />
				<span>Trigger Shape</span>
				<select bind:value={shape}>
					{#each shapeOptions as option}
						<option value={option}>
							{option}
						</option>
					{/each}
				</select>
				<span class="mt-3">Config name</span>
				<input bind:value={configName} />
				<div
					class="text-center rounded-md shadow-lg border-2 mx-auto w-fit py-2 mt-2"
					on:click={saveConfig}
				>
					Save Configuration
				</div>
				{#each configs as config}
					<div
						class="flex flex-col cursor-pointer"
						on:click={() => {
							loadConfig(config);
						}}
					>
						<span class="font-semibold">
							{config.name}
						</span>
						<span class="text-sm">
							Page Background: {config.backgroundColor}
						</span>
						<span class="text-sm">
							Trigger Shape: {config.shape}
						</span>
						<span class="text-sm">
							Trigger Color: {config.shapeColor}
						</span>
					</div>
				{/each}
			</div>
		</div>
	</div>
</template>

<style>
	input {
		@apply bg-white border-blue-200 border-2;
	}
</style>
