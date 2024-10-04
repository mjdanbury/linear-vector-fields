<script>
	import P5 from 'p5-svelte';

	let res = 20;
	let backRes = 5;
	let width = 400;
	let height = 400;
	let cols = width / res;
	let rows = height / res;
	let backCols = width / backRes;
	let backRows = height / backRes;
	let a11 = 1;
	let a12 = -1;
	let a21 = 0;
	let a22 = -1;
	let normalize = true;
	let eigensharpness = 400; // New parameter to control the sharpness of the distinction
	let nullsharpness = 1;

	const sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(400, 400);
			p5.colorMode(p5.HSB, 360, 100, 100);
		};

		const getColor = (v, Av) => {
			// Calculate the cosine of the angle between v and Av
			let cosAngle = (v.x * Av.x + v.y * Av.y) / (v.mag() * Av.mag());

			// Take the absolute value to treat parallel and antiparallel vectors the same
			let absCosAngle = Math.abs(cosAngle);

			// Apply a power function to create a sharper distinction
			let sharpened = Math.pow(absCosAngle, eigensharpness);

			// Map |cos(angle)| to a color
			// We'll use a gradient from white (perpendicular) to a saturated color (parallel/antiparallel)
			let hue = 20; // Blue hue
			let saturation = sharpened * 20;
			let brightness = 100 * (1 - 0.4 * Math.exp(-1 * nullsharpness * Av.mag()));

			return p5.color(hue, saturation, brightness);
		};

		// p5 variables and functions need to be prefixed since we have not globally imported the namespace. Other than that, everything else works the same :~)
		p5.draw = () => {
			p5.createCanvas(400, 400);
			p5.background(0, 0, 100); // White background in HSB

			// Calculate the canvas center
			let centerX = p5.width / 2;
			let centerY = p5.height / 2;

			// Draw colored background
			for (let i = 0; i < backCols; i++) {
				for (let j = 0; j < backRows; j++) {
					let v = p5.createVector(i - backCols / 2 + 0.5, -1 * (j - backRows / 2 + 0.5));
					let Av = p5.createVector(a11 * v.x + a12 * v.y, a21 * v.x + a22 * v.y);

					let x = i * backRes;
					let y = j * backRes;

					p5.noStroke();
					p5.fill(getColor(v, Av));
					p5.rect(x, y, backRes, backRes);
				}
			}

			// Draw the axes
			p5.push();
			p5.stroke(80); // Light grey color for axes
			p5.strokeWeight(2);

			// Draw x-axis
			p5.line(0, centerY, p5.width, centerY);

			// Draw y-axis
			p5.line(centerX, 0, centerX, p5.height);

			// Optional: Add arrow tips to axes
			let arrowSize = 10;

			// X-axis arrow tip
			p5.line(p5.width, centerY, p5.width - arrowSize, centerY - arrowSize / 2);
			p5.line(p5.width, centerY, p5.width - arrowSize, centerY + arrowSize / 2);

			// Y-axis arrow tip
			p5.line(centerX, 0, centerX - arrowSize / 2, arrowSize);
			p5.line(centerX, 0, centerX + arrowSize / 2, arrowSize);

			p5.pop();

			for (let i = 0; i < cols + 1; i++) {
				for (let j = 0; j < rows + 1; j++) {
					let v = p5.createVector(i - cols / 2, -1 * (j - rows / 2));
					let Av = p5.createVector(a11 * v.x + a12 * v.y, a21 * v.x + a22 * v.y);

					// Calculate the starting point of the vector
					let x = centerX + (i - cols / 2) * res;
					let y = centerY + (j - rows / 2) * res;

					let endX = x;
					let endY = y;

					// Scale Av to enhance visability
					if (normalize) {
						Av.setMag(res / 2);
						endX = x + Av.x;
						endY = y - Av.y;
					} else {
						endX = x + Av.x * res;
						endY = y + Av.y * res;
					}

					// Draw the main line of the arrow
					p5.stroke(0);
					p5.strokeWeight(1);
					p5.line(x, y, endX, endY);

					if (Av.mag() > 0) {
						// Calculate the angle of the arrow
						let angle = Av.heading();

						// Length of the arrowhead lines
						let arrowSize = 3;

						// Calculate arrowhead points
						let arrowAngle = p5.PI / 6; // 30 degrees
						let x1 = endX - arrowSize * p5.cos(angle - arrowAngle);
						let y1 = endY + arrowSize * p5.sin(angle - arrowAngle);
						let x2 = endX - arrowSize * p5.cos(angle + arrowAngle);
						let y2 = endY + arrowSize * p5.sin(angle + arrowAngle);

						// Draw arrowhead
						p5.line(endX, endY, x1, y1);
						p5.line(endX, endY, x2, y2);
					}
				}
			}
		};
	};
</script>

<div class="body">
	<a href="/">Home</a>
	<h1>A Static Picture of Linear Transformations</h1>
	<button on:click={() => (normalize = !normalize)}>
		{#if normalize}
			Show raw vectors
		{:else}
			Normalize vectors
		{/if}
	</button>
	<div class="sketch"><P5 {sketch} /></div>
	<div class="controls">
		Transformation Matrix
		<div class="matrix">
			<input type="number" bind:value={a11} />
			<input type="number" bind:value={a12} />
			<input type="number" bind:value={a21} />
			<input type="number" bind:value={a22} />
		</div>
	</div>
	<p>
		TODO - It could be nice to scale the lengths of the vetors by tanh(scaleFactor*norm(Av)). To
		emphasize the fact that these aren't really of uniform length.
	</p>
	<p>
		TODO - Right now, the eigen/null space code seems to be doing the right thing but the vector
		field is incorrect.
	</p>
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
		text-justify: center;
		margin-top: 25px;
	}
	.matrix {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 1fr 1fr;
	}
</style>
