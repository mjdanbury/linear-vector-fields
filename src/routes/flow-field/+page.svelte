<script>
	// import { evaluate } from 'mathjs'; Postponing for now
	import LinearVectorField from './LinearVectorField.svelte';

	let a11 = $state(1);
	let a12 = $state(-1);
	let a21 = $state(0);
	let a22 = $state(-1);
	let highlight = $state(false);
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
		sick. As would decompositions like SVD, Similarity transformations, adjoints, inverses,
		transposes, etc.
	</p>

	<p>
		Arguably, a lot of the of the value of this representation is how it would allow people to get
		the "gist" of what linear transformations, matrix factorizations, etc. do. The structures and
		the precision required to prove theorems will always be work to develop but seeing the point is
		priceless.
	</p>
	<button on:click={() => (highlight = !highlight)}>
		{#if highlight}
			Remove highlights
		{:else}
			Highlight eigenspaces
		{/if}
	</button>
	<div class="sketch"><LinearVectorField {a11} {a12} {a21} {a22} {highlight} /></div>
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
