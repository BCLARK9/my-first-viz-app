<script lang="ts">
	import * as d3 from 'd3';

	let myName = 'Matt';
	let width = 300;
	let height = 300;
	let newx = 0;
	let newy = 0;
	let newcolor = '#AABBCC';

	let myData = [
		{ x: 1.6, y: 4.0, colors: ['red', 'blue', 'green'] },
		{ x: 1.1, y: 0.5, colors: ['green', 'red', 'blue'] },
		{ x: 2.1, y: 1.9, colors: ['bluesteel', 'redbrick', 'brown'] },
		{ x: 4.8, y: 3.1, colors: ['yellow', 'grey', 'black'] }
	];

	let chosenColorIndex = 0;

	function addNewDatapoint(event: e) {
		const newPoint = {
			x: newx,
			y: newy,
			color: newcolor,
			colorb: newcolor
		};
		myData = [newPoint, ...myData];
	}

	let totalArea: number;
	$: {
		totalArea = width * height;
		console.log('Changing totalArea to ', totalArea);
	}

	let doubleWidth: number;

	$: {
		doubleWidth = width * 2;
		console.log('Changing doubleWidth to ', doubleWidth);
	}

	$: {
		console.log('Changing chosenColorIndex to ', chosenColorIndex);
		d3.select('#mySvg')
			.selectAll('circle')
			.data(myData)
			.transition()
			.style('fill', (d) => d.colors[chosenColorIndex]);
	}
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
<p>Hi there, {myName}!</p>

<p>
	Total pixels available: {totalArea}. And, double width is {doubleWidth}. Chosen index is {chosenColorIndex}.
</p>
<p><input type="text" bind:value={myName} /></p>
<p>
	<select bind:value={chosenColorIndex}>
		<option value="0">First</option>
		<option value="1">Second</option>
		<option value="2">Third</option>
	</select>
</p>
<p>
	<input type="range" min="100" max="500" step="1" name="width" bind:value={width} /><label
		for="width">SVG Width</label
	>
</p>
<p>
	<input type="range" min="100" max="500" step="1" name="height" bind:value={height} /><label
		for="height">SVG Height</label
	>
</p>

<svg id="mySvg" {width} {height} style="border: 1px solid black;">
	{#each myData as dataPoint}
		<circle
			cx={(dataPoint.x * width) / 5.0}
			cy={(dataPoint.y * height) / 5.0}
			r="10"
			style="fill: {dataPoint.color};"
		></circle>
	{/each}
</svg>

<p>
	<input type="range" min="0" max="5" step="0.1" bind:value={newx} name="newx" /><label for="newx"
		>New X Point</label
	>
	<input type="range" min="0" max="5" step="0.1" bind:value={newy} name="newy" /><label for="newy"
		>New Y Point</label
	>
	<input type="color" name="newcolor" bind:value={newcolor} /><label for="newcolor">New Color</label
	>
	<button on:click={addNewDatapoint}>Add new point</button>
</p>
