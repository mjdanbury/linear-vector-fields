<script>
	import P5 from 'p5-svelte';
	// import { evaluate } from 'mathjs'; Postponing for now

	let fieldResolution = 20; // Expose, default to 20
	let highlightResolution = 5; // Expose, default to 5
	let width = 400; // Expose, default to 400
	let height = 400; // Expose, default to 400
	let cols = width / fieldResolution;
	let rows = height / fieldResolution;
	let highlightCols = width / highlightResolution;
	let highlightRows = height / highlightResolution;
	let a11 = 1; // Expose
	let a12 = -1; // Expose
	let a21 = 0; // Expose
	let a22 = -1; // Expose
	let highlight = false;
	let highlightHue = 80; // Expose, default to 80 (light green in HSB)
	let eigenSharpness = 400;
	let nullSharpness = 1;
	let lengthScale = 7; // Expose, default to 7
	let arrowSize = 10;

	const sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(width, height);
			p5.colorMode(p5.HSB, 360, 100, 100);
		};

		const getColor = (v, Av) => {
			// Calculate the cosine of the angle between v and Av
			let cosAngle = (v.x * Av.x + v.y * Av.y) / (v.mag() * Av.mag());

			// Take the absolute value to treat parallel and antiparallel vectors the same
			let absCosAngle = Math.abs(cosAngle);

			// Apply a power function to create a sharper distinction
			let sharpened = Math.pow(absCosAngle, eigenSharpness);

			// Map |cos(angle)| to a color
			// We'll use a gradient from white (perpendicular) to a saturated color (parallel/antiparallel)

			let saturation = sharpened * 15;
			let brightness = 100;
			// let brightness = 100 * (1 - 0.4 * Math.exp(-1 * nullSharpness * Av.mag()));

			return p5.color(highlightHue, saturation, brightness);
		};

		// p5 variables and functions need to be prefixed since we have not globally imported the namespace. Other than that, everything else works the same :~)
		p5.draw = () => {
			p5.createCanvas(width, height);
			p5.background(0, 0, 100); // White background in HSB

			// Calculate the canvas center
			let centerX = p5.width / 2;
			let centerY = p5.height / 2;

			if (highlight) {
				// Draw colored background
				for (let i = 0; i < highlightCols; i++) {
					for (let j = 0; j < highlightRows; j++) {
						let v = p5.createVector(
							i - highlightCols / 2 + 0.5,
							-1 * (j - highlightRows / 2 + 0.5)
						);
						let Av = p5.createVector(a11 * v.x + a12 * v.y, a21 * v.x + a22 * v.y);

						let x = i * highlightResolution;
						let y = j * highlightResolution;

						p5.noStroke();
						p5.fill(getColor(v, Av));
						p5.rect(x, y, highlightResolution, highlightResolution);
					}
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
					let lengthAv = Av.mag();

					// Calculate the starting point of the vector
					let x = centerX + (i - cols / 2) * fieldResolution;
					let y = centerY + (j - rows / 2) * fieldResolution;

					let endX = x;
					let endY = y;

					// Scale Av to enhance visability
					Av.setMag((Math.tanh(lengthAv / lengthScale) * fieldResolution) / 2);
					endX = x + Av.x;
					endY = y - Av.y;

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
	<h1>Linear Algebra in a Single Picture</h1>
	<h3>Alternative titles:</h3>
	<ul>
		<li>A Quick, Visual Introduction to Linear Algebra</li>
		<li>A Complementary Picture of Linear Algebra</li>
		<li>A New Picture of Linear Transformations</li>
		<li>Linear Vector Fields - A Visual Playground for learning Linear Algebra</li>
	</ul>
	<p>
		One of the things that this picture shows is what "Linearity" means. Scaling an input (moving
		along any line through the origin) simply scales the output (sign + magnitude of output vector
		changes).
	</p>
	<p>
		This is in contrast to a nonlinear transformation of the plane (show picture of a nonlinear
		vector field) where things can go off in an entirely different direciton as your input scales.
	</p>
	<p>
		All finite dimensional vector spaces are isomorphic to R^n so this is a great place to build
		intuition.
	</p>
	<p>
		What else can this picture get us. In almost every introduction to linear algebra I've
		encountered. The trace is sheepishly defined as "the sum of the main diagonal of a matrix". This
		presentation shows how the trace appears naturally as the divergence of the vector field
		associated with a linear transformation
	</p>
	<p>
		Grant's animations are still the best when it comes to getting intuition about how a single
		linear transformation plays out. Where this one excels I think is building intution about the
		space of all linear transformations, and how they relate to their matrix representations.
	</p>

	<p>
		When learning math, you sometimes get the advice that you just need to keep reading. Sometimes,
		this is a good and practical option but I don't think it really builds the skills you'd need as
		a research or careful thinker of any breed.
	</p>
	<p>
		What I like about this is that by playing around with it, you can generate questions that could
		"chart a course" for the slog that is an intermediate, proof-based linear algebra class. You see
		patterns and hopefully crave to distill them into rigorous theorems about the structure inherent
		to linear maps.
	</p>
	<p>
		Even better, these pictures allow you to explore questions that could whet your appetite for a
		followup course in Lie Theory (sometimes reffered to as Matrix Groups). Thats amazing.
	</p>
	<p>
		Using small multiples to see how an algorithm like row reduction plays out would be fucking
		sick. As would decompositions like SVD, Similarity transformations, etc.
	</p>

	<button on:click={() => (highlight = !highlight)}>
		{#if highlight}
			Remove highlights
		{:else}
			Highlight eigenspaces
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
		<h4>Determinant: {a11 * a22 - a12 * a21}</h4>
		<h4>Trace: {a11 + a22}</h4>
	</div>
	<p>
		TODO - Incorporating the javascript math library to be able to write things like sqrt, pi, etc.
		would be a nice to have.
	</p>
	<p>
		TODO - Perhaps it would be cool to sync the matrix input to the matrix vector product written
		out so you can see why certain components are/arent changing.
	</p>
	<p>TODO - Watch Lie Theory series in full to brainstorm for writeup.</p>
	<p>
		TODO - There's no reason I couldn't implement Grant's animation for single transformations. This
		would be quite a squeeze tho. Is the juice worth it? Perhaps you could have a little play button
		in one of the corners and it would and it could bounce loop until paused. You wouldn't need to
		animate the eigenspace highlights as those remains static throughout a transformation :^)
		ACTUALLY - Idk if this would be all that great. Input density is uniform but output density
		isn't necessarily so. Space can be stretched / squished. But maybe that's a good thing.
	</p>
	<p>
		TODO - A really neat thing to do would be to have a side by side view of a matrix and its
		inverse. Play with one and see how the inverse changes. You could do the same thing for a matrix
		and it's SVD (assuming that exists..)
	</p>
	<p>TODO - Making the colors / fonts / layout a little more Tuftean would be pretty cool.</p>
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
