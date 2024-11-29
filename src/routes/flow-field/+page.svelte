<script>
	// import { evaluate } from 'mathjs'; Postponing for now
	import LinearVectorField from './LinearVectorField.svelte';

	let A = $state([
		[1, -1],
		[0, -1]
	]);
	let highlight = $state(false);
	let selected = $state();
</script>

<div class="body">
	<!-- <a href="/">Home</a> -->
	<!-- <p class="date">November 2024</p> -->
	<h1>Linear Algebra In A Single Picture</h1>
	<div class="sketch">
		<div class="field">
			<LinearVectorField {A} />
		</div>
	</div>

	<p><u>Matthew Danbury</u></p>
	<!-- <p>November 2024</p> -->

	<p>
		As a math student, I sometimes felt that the definitions I was handed were undermotivated —
		answers to questions I'd yet to ask. Oftentimes, I would get past these roadblocks by just
		"stomaching it" and trusting that things would make more sense as I read further along in the
		textbook and worked some of the exercises. Most of the time they did.
	</p>

	<p>
		While this learning method works pretty reliably in the classroom, it utterly fails to
		generalize to beyond it. Real life problems don't come with some textbook you can just read
		further along in. Most anything in life worth doing is novel in some way or another and doing it
		means leaning on your own careful thinking to frame the problem in a way that makes sense to
		you, and develop a plan of attack. This sort of careful thinking is a skill unto itself, and one
		that can be honed with practice. Here, I present a way to offer linear algebra students such an
		opportunity.
	</p>

	<p>
		This might not be the best way to understand what an <em>individual</em> linear transformation
		does per se but I think it is an extaordinarily good way to look at linear transformations
		<em>as a whole</em>, and generate the kinds of questions that could chart out a first or second
		course on the topic.
	</p>

	<p>
		A course in Linear Algebra typically starts with defining what a vector space is, and proceedes
		to charactarize transformations between them.
	</p>
	<p>
		This picture is created by the following "rule" at each point in space, draw the vector that
		that point is mapped to under A. Some caveats we only sample a finite number of points, and we
		scale the vectors down to improve visibility (I used tanh to map all magnitudes into a finite
		interval.)
	</p>
	<p>
		It has been noted that the way math is discovered and the way math is taught are often nothing
		alike. Research mathematicians tackle problems through concrete examples, which can help them
		develop and hone useful abstractions and strategies for proving more general theorems. Yet open
		any math textbook, and you'll be presented with a slew of abstract definitions and theorems,
		followed by concrete exercises. On the other hand, research procedes by toying around with
		concrete examples to tease out and hone, what the most useful abstractions could be.
	</p>
	<p>
		I'm sure some of this is unavoidable. If you want to move at an effective pace, you can't be
		expected to completely rediscover entire fields that. The downside is that as a student, I would
		often feel that the definitions I was handed were undermotivated. Answers to questions I hadn't
		yet asked.
	</p>
	<p>
		Yet when it comes to linear transformations, they are left with matrices — a computational
		object — and pushing numbers arond with their pencils.
	</p>

	<p>
		For positive determinant matrices, making an orbit around the origin, either the head or the
		tail of the arrows are consistently pointed outwards. For negative determinant matrices, they
		flip. Twice.
	</p>
	<p>###################################</p>
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
			<input type="range" min="-6" max="6" step="1" bind:value={A[0][0]} />
		{/if}
		{#if selected == 'a12'}
			<input type="range" min="-6" max="6" step="1" bind:value={A[0][1]} />
		{/if}
		{#if selected == 'a21'}
			<input type="range" min="-6" max="6" step="1" bind:value={A[1][0]} />
		{/if}
		{#if selected == 'a22'}
			<input type="range" min="-6" max="6" step="1" bind:value={A[1][1]} />
		{/if}

		<h4>Determinant: {A[0][0] * A[1][1] - A[0][1] * A[1][0]}</h4>
		<h4>Trace: {A[0][0] + A[1][1]}</h4>
	</div>
	<div class="footer">Matthew Danbury is a freelance data interactives developer.</div>
</div>

<style>
	.body {
		/* background: red; */
		max-width: 600px;
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
	.date {
		text-align: end;
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
			border-style: solid;
			border-width: 1px;
		}
	}
</style>
