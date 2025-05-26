<script>
	import P5 from 'p5-svelte';

	let {
		A,
		width = 300,
		height = 300,
		fieldResolution = 25,
		vectorScale = 3,
		highlight = false,
		highlightHue = 80
	} = $props();

	// Drawing params (derived / unexposed)
	const cols = width / fieldResolution;
	const rows = height / fieldResolution;
	const highlightResolution = 5;
	const highlightCols = width / highlightResolution;
	const highlightRows = height / highlightResolution;
	const eigenSharpness = 400;
	const nullSharpness = 1;
	const arrowheadSize = 10;

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

			// TODO - Null space of a matrix is always an eigenspace with eigenvalue 0. The current highlights capture this inconsistently.
			let saturation = sharpened * 30;
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
						let Av = p5.createVector(A[0][0] * v.x + A[0][1] * v.y, A[1][0] * v.x + A[1][1] * v.y);

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
			p5.stroke(50); // Light grey color for axes
			p5.strokeWeight(1);
			p5.drawingContext.setLineDash([3, 6]);

			// Draw x-axis
			p5.line(0, centerY, p5.width, centerY);

			// Draw y-axis
			p5.line(centerX, 0, centerX, p5.height);

			// X-axis arrow tip
			// p5.line(p5.width, centerY, p5.width - arrowheadSize, centerY - arrowheadSize / 2);
			// p5.line(p5.width, centerY, p5.width - arrowheadSize, centerY + arrowheadSize / 2);

			// Y-axis arrow tip
			// p5.line(centerX, 0, centerX - arrowheadSize / 2, arrowheadSize);
			// p5.line(centerX, 0, centerX + arrowheadSize / 2, arrowheadSize);

			p5.pop();

			for (let i = 0; i < cols + 1; i++) {
				for (let j = 0; j < rows + 1; j++) {
					let v = p5.createVector(i - cols / 2, -1 * (j - rows / 2));
					let Av = p5.createVector(A[0][0] * v.x + A[0][1] * v.y, A[1][0] * v.x + A[1][1] * v.y);
					let lengthAv = Av.mag();

					// Calculate the starting point of the vector
					let x = centerX + (i - cols / 2) * fieldResolution;
					let y = centerY + (j - rows / 2) * fieldResolution;

					let endX = x;
					let endY = y;

					// Scale Av to enhance visability
					Av.setMag((Math.tanh(lengthAv / vectorScale) * fieldResolution) / 2);
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
						let arrowSize = 3; //* Math.tanh(lengthAv / vectorScale);

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

<P5 {sketch} />
