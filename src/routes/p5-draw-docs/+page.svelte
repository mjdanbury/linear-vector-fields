<script>
	import P5 from 'p5-svelte';
	let a = 1;
	let b = 0;
	let c = 0;
	let d = 1;

	const sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(400, 400);
			p5.background('#05abac');
		};

		// p5 variables and functions need to be prefixed since we have not globally imported the namespace. Other than that, everything else works the same :~)
		p5.draw = () => {
			p5.circle(p5.mouseX, p5.mouseY, 100);
		};

		p5.doubleClicked = () => {
			p5.fill('deeppink');
		};
	};
</script>

<div class="body">
	<a href="/">Home</a>
	<h1>p5 draw()</h1>
	<div class="sketch">
		<P5 {sketch} debug />
	</div>
	<div class="slider"></div>
	<div class="slider"></div>
	<div class="slider"></div>
	<div class="slider"></div>
</div>
