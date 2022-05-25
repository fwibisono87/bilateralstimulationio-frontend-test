<script lang="ts">
	export let isCircle = true;
	export let size = '50px';
	export let color = 'red';
    export let additionalClass = ''
</script>

<template>
	<div
        class={additionalClass}
		class:rounded-full={isCircle === true}
		style="background-color: {color}; width: {size}; height: {size}"
	/>
</template>
