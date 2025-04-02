<script>
	// import { evaluate } from 'mathjs'; Postponing for now
	import LinearVectorField from './LinearVectorField.svelte';

	let A = $state([
		[1, -1],
		[-1, -1]
	]);
	let highlight = $state(false);
	let selected = $state();
</script>

<div class="body">
	<h1>A Visual Primer on Linear Algebra</h1>
	<div class="sketch">
		<div class="hero-lg">
			<LinearVectorField {A} width={550} />
		</div>
		<div class="hero-md">
			<LinearVectorField {A} width={450} />
		</div>
		<div class="hero-sm">
			<LinearVectorField {A} width={350} />
		</div>
	</div>
	<p>By <a href="https://matthewdanbury.info">Matthew Danbury</a></p>

	<p>
		What are linear transformations? What secrets do they hold? Getting deep, satisfying answers to
		these questions is the point of learning linear algebra – the plot of the story if you will.
	</p>

	<p>
		Getting to the end of this story will take work. Hard work. Why should you put in the effort?
	</p>

	<p>
		Instead of taking the usual tack of touting linear algebra's utility, I want to give you a sense
		of the "big picture" of the subject and its mysteries.
	</p>

	<p>
		My hope is that by doing this early on, you'll come into your learning with some urgent
		questions you want resolved and a general sense of where things are heading.
	</p>

	<p>
		Hopefully then, the definitions and theorems you encounter will seem a lot better motivated.
		Reasonble even.
	</p>

	<p>
		We'll do this by treating linear transformations as black boxes and seeing what we can learn
		about them simply by getting our hands dirty.
	</p>

	<h2>Vector Spaces</h2>

	<p>
		If linear transformations are the "plot" of linear algebra, vectors and vector spaces are the
		characters and setting. Before we can get anywhere, we'll need to introduce you to them.
	</p>

	<p>
		Up until now, If you had an equation with multiple variables you would view them as living in
		separate universes. You could picture this as each living in it's own copy of the real number
		line.
	</p>

	<p>But the different variables in our equations often have something to do with one another</p>

	<p>
		Rather than have two different things, you should have one thing. <em>What is that thing?</em>
	</p>

	<h2>Visualization</h2>

	<p>
		Basically. we have a transformation - something that creates new vectors from old. we don't know
		anything else about it. how can we explore it? one way would be to start seeing what it does on
		different values. We *could* try it on a handful of values but I'll "suggest" that we could be
		more systematic about it. If we knew what it did with every value, we would say we would know
		the full transformation. Obviously, testing every pair of real numbers would take too long. So
		we'll compromise by testing a regular grid of points instead to see what sort of picture it can
		give us (sort of like a digital camera).
	</p>

	<p>
		Right now you should be thinking that if the information content of the transformation is four
		parameters and we're using dozens of probes, perhaps we're oversampling? Yes and no. You'd be
		right in this case but generally speaking, "simple" generators can produce outputs that are
		difficult to learn.
	</p>

	<h2>Scavenger Hunt</h2>
	<p>
		If you prefer to just noodle around. Do that. If you'd prefer some more guided exploration see
		if you can find the following transformations:
	</p>

	<h2>Patterns</h2>

	<p>
		One of the things that this picture shows is what "Linearity" means. Scaling an input (moving
		along any line through the origin) simply scales the output (sign + magnitude of output vector
		changes). Also, the effect of adding inputs is simply adding outputs, as you can verify by
		looking at
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
		encountered. The trace is defined as "the sum of the main diagonal of a matrix". This
		presentation shows how the trace appears naturally as the divergence of the vector field
		associated with a linear transformation
	</p>

	<h2>Gaussians</h2>
	<p>
		Linear transformations map ellipsoids to ellipsoids. Put another way, every ellipsoid centered
		at the origin corresponds to O(n) worth of linear transformations. Specifically, every ellipsoid
		centered at the origin accounts for O(n) worth of linear transformations. (why?)
	</p>
	<p>Gaussians are ellipsoidal!!</p>

	<h2>Inverses</h2>

	<p>"Can I undo this?" is a very natural question to ask about a transformation</p>

	<p>
		Q: what sorts of matrices lack an inverse? What do they look like? Can you explain what's going
		on?
	</p>

	<!-- TODO - Interactive where you control the knobs of the matrix. 2x2 inverse is plotted as well -->

	<h2>Compositions</h2>
	<p>
		What if I do one transformation after another? Since the result should be another vector, the
		composition should itself be a transformation
	</p>

	<p>
		This begs the question, are there clever ways to "decompose" a transformation into other
		transformations? You tell me!
	</p>

	<p>
		In algebra, when we have an operation on two things that produces another thing, we call it
		multiplication. If the operation commutes, we call it addition. Convince yourself that
		composition of transformations is multiplication. Does it commute? (Yes/No/Sometimes) Can you
		define a rule for this multiplication?
	</p>

	<h2>Determinats</h2>

	<p>
		For positive determinant matrices, making an orbit around the origin, either the head or the
		tail of the arrows are consistently pointed outwards. For negative determinant matrices, they
		flip. Twice.
	</p>

	<p>###################################</p>

	<p>
		Using small multiples to see how an algorithm like row reduction plays out would be fucking
		sick. As would decompositions like SVD, Similarity transformations, adjoints, inverses,
		transposes, etc.
	</p>

	<p>
		Arguably, a lot of the of the value of this representation is how it would allow people to get
		the "gist" of what linear transformations, matrix factorizations, etc. do. The structures and
		the precision required to prove theorems will always be work to develop but seeing the point is
		priceless.
	</p>
	<p>
		One natural observation is that the arrows "follow" one another . A reasonable question might be
		to ask if there is any operation that corresponds to.
	</p>
	<p>
		A good takeaway from a linear algebra course is that linear transformations do a finite number
		of things. They can stretch and squish space. They can rotate it. They can flip/invert it. And
		they can project. And any combinations therein. But thats it.
	</p>
	<button onclick={() => (highlight = !highlight)}>
		{#if highlight}
			Remove highlights
		{:else}
			Highlight eigenspaces
		{/if}
	</button>
	<div class="sketch">
		<div class="field">
			<LinearVectorField {A} {highlight} />
		</div>
	</div>
	<div class="controls">
		<p>Transformation Matrix</p>
		<button
			onclick={() => {
				selected = 'a11';
			}}>{A[0][0]}</button
		>
		<button
			onclick={() => {
				selected = 'a12';
			}}>{A[0][1]}</button
		>
		<br />
		<button
			onclick={() => {
				selected = 'a21';
			}}>{A[1][0]}</button
		>
		<button
			onclick={() => {
				selected = 'a22';
			}}>{A[1][1]}</button
		>
		<br />
		{#if !selected}
			<input type="range" disabled={true} />
		{/if}
		{#if selected == 'a11'}
			<input type="range" min="-6" max="6" step="0.5" bind:value={A[0][0]} />
		{/if}
		{#if selected == 'a12'}
			<input type="range" min="-6" max="6" step="0.5" bind:value={A[0][1]} />
		{/if}
		{#if selected == 'a21'}
			<input type="range" min="-6" max="6" step="0.5" bind:value={A[1][0]} />
		{/if}
		{#if selected == 'a22'}
			<input type="range" min="-6" max="6" step="0.5" bind:value={A[1][1]} />
		{/if}

		<h4>Determinant: {A[0][0] * A[1][1] - A[0][1] * A[1][0]}</h4>
		<h4>Trace: {A[0][0] + A[1][1]}</h4>
	</div>
	<div class="footer">
		🚧 This draft is a work in progress. Thank you for bearing with the rough edges. 🚧
	</div>
</div>

<style>
	.body {
		/* background: red; */
		max-width: 650px;
		margin: 0 auto;
		font-size: 16px;
		font-family: 'Noto Sans Mono', monospace;
		line-height: 1.45;
		font-weight: 100;
		text-rendering: optimizeLegibility;
	}
	h1 {
		padding-top: 15px;
		border-top: 1px solid black;
	}

	.sketch {
		/* background: orange; */
		display: flex;
		justify-content: space-around;
	}
	.controls {
		text-justify: center;
		margin-top: 25px;
	}
	.field {
		/* background: yellow; */
		margin: 15px;
		padding: 15px;
		border-style: solid;
		border-width: 1px;
	}

	.hero-lg {
		/* background: yellow; */
		/* margin: 15px; */
		padding: 15px;
		/* border-style: solid; */
		/* border-width: 1px; */
	}
	.hero-md {
		display: none;
	}
	.hero-sm {
		display: none;
	}

	@media (max-width: 650px) {
		.body {
			/* background: blue; */
			padding-left: 20px;
			padding-right: 20px;
		}
		.field {
			/* background: yellow; */
			margin: 5px;
			padding: 5px;
		}
		.hero-lg {
			display: none;
		}
		.hero-md {
			display: contents;
		}
	}
	@media (max-width: 500px) {
		.hero-md {
			display: none;
		}
		.hero-sm {
			display: contents;
		}
	}
</style>
