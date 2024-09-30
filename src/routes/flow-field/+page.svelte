<script>
	import P5 from 'p5-svelte';

	let res = 20;
	let width = 400;
	let height = 400;
	let cols = width / res;
	let rows = height / res;
	let a11 = 2;
	let a12 = 2;
	let a21 = 1;
	let a22 = 1;

	const sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(400, 400);
		};

		// p5 variables and functions need to be prefixed since we have not globally imported the namespace. Other than that, everything else works the same :~)
		p5.draw = () => {
			p5.createCanvas(400, 400);
			for (let i = 0; i < cols; i++) {
				for (let j = 0; j < rows; j++) {
					let v = p5.createVector(i - cols / 2, j - cols / 2);
					// v.setMag(res * 0.5);
					let Av = p5.createVector(a11 * v.x + a12 * v.y, a21 * v.x + a22 * v.y);
					// let mag = Av.mag();
					// Av.setMag(res / 2);
					let x = i * res + res / 2;
					let y = j * res + res / 2;
					p5.strokeWeight(1);
					p5.line(x, y, x + Av.x, y + Av.y);
					p5.circle(x + Av.x, y + Av.y, 2);
				}
			}
		};
	};
</script>

<div class="body">
	<a href="/">Home</a>
	<h1>Linear Vector Fields</h1>
	<div class="sketch"><P5 {sketch} /></div>
	<div class="controls">
		<div class="sliders-container">
			<div class="slider">
				a11
				<input type="range" bind:value={a11} min="-5" max="5" step="0.05" />
				<!-- {a11} -->
			</div>
			<div class="slider">
				a12
				<input type="range" bind:value={a12} min="-5" max="5" step="0.05" />
				<!-- {a12} -->
			</div>
			<div class="slider">
				a21
				<input type="range" bind:value={a21} min="-5" max="5" step="0.05" />
				<!-- {a21} -->
			</div>
			<div class="slider">
				a22
				<input type="range" bind:value={a22} min="-5" max="5" step="0.05" />
				<!-- {a22} -->
			</div>
		</div>

		<div class="matrix">
			<p>{a11}</p>
			<p>{a12}</p>
			<p>{a21}</p>
			<p>{a22}</p>
		</div>
	</div>
</div>

<style>
	.body {
		/* background: red; */
		width: 600px;
		margin: 0 auto;
	}
	.sketch {
		display: flex;
		justify-content: center;
	}
	.controls {
		display: flex;
		justify-content: space-around;
	}
	.matrix {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 1fr 1fr;
		width: 100px;
		height: 30px;
	}
</style>
