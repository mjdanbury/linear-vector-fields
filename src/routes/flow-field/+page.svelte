<script>
	import P5 from 'p5-svelte';

	let res = 20;
	let width = 400;
	let height = 400;
	let cols = width / res;
	let rows = height / res;
	let a11 = 1;
	let a12 = 0;
	let a21 = 0;
	let a22 = 1;

	const sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(400, 400);
			p5.colorMode(p5.HSB);
		};

		// p5 variables and functions need to be prefixed since we have not globally imported the namespace. Other than that, everything else works the same :~)
		p5.draw = () => {
			// for (let y = 0; y < height; y++) {
			// 	for (let x = 0; x < width; x++) {
			// 		let dx = x - width / 2;
			// 		let dy = y - height / 2;
			// 		let distance = p5.sqrt(dx * dx + dy * dy);
			// 		let angle = p5.atan2(dy, dx);
			// 		let hue = p5.map(angle, -p5.PI, p5.PI, 0, 360);

			// 		p5.stroke(hue, 100, 100);
			// 		p5.point(x, y);
			// 	}
			// }
			p5.createCanvas(400, 400);
			for (let i = 0; i < cols; i++) {
				for (let j = 0; j < rows; j++) {
					let v = p5.createVector(i - cols / 2, j - cols / 2);
					let Av = p5.createVector(a11 * v.x + a12 * v.y, a21 * v.x + a22 * v.y);
					// Av.setMag(res / 2);
					let x = i * res + res / 2;
					let y = j * res + res / 2;

					let endX = x + Av.x;
					let endY = y + Av.y;
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
						let y1 = endY - arrowSize * p5.sin(angle - arrowAngle);
						let x2 = endX - arrowSize * p5.cos(angle + arrowAngle);
						let y2 = endY - arrowSize * p5.sin(angle + arrowAngle);

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
	<h1>Linear Vector Fields</h1>
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
	}
	.matrix {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 1fr 1fr;
	}
</style>
