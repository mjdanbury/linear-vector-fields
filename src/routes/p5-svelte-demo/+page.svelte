<script>
	import P5 from 'p5-svelte';
	let width = 55;
	let height = 55;

	const sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(400, 400);
		};

		p5.draw = () => {
			p5.ellipse(p5.width / 2, p5.height / 2, width, height);
		};
	};
</script>

<div class="body">
	<a href="/">Home</a>
	<h1>p5 Svelte</h1>
	<div class="sketch">
		<P5 {sketch} debug />
	</div>

	<div class="slider">
		Width
		<input type="range" bind:value={width} min="100" max="1000" step="0.01" />
		{width}
	</div>
	<div class="slider">
		Height
		<input type="range" bind:value={height} min="100" max="1000" step="0.01" />
		{height}
	</div>
</div>

<style>
	.body {
		/* background: red; */
		width: 600px;
		margin: 0 auto;
		display: flex;
		flex-direction: column;
		align-self: center;
		justify-content: center;
	}
	.slider {
		/* background: orange; */
		display: flex;
		justify-content: center;
		align-items: center;
		margin: 10px;
	}
	.sketch {
		/* background-color: yellow; */
		display: flex;
		justify-content: center;
	}
</style>
