<script>
	import { tm_trace_to_image, tm_explore } from '$lib/tm';

	export let exploreMode;
	export let machine;
	export let initial_tape;
	export let tapeWidth;
	export let nbIter;
	export let origin_x;
	export let headStyle;

	export let machineName;

	let canvas;

	const drawRect = (context) => {
		context.fillStyle = 'black';
		context.fillRect(0, 0, canvas.width, canvas.height);
		context.fill();
	};

	let drawCleanup;
	function draw() {
		if (drawCleanup) drawCleanup();

		// Update the width of the canvas inside of `draw`, because otherwise
		// it gets updated _afterward_ by the Svelte update loop, which will
		// blank out the canvas entirely.
		canvas.width = exploreMode ? 800 : 400;

		if (!machine) {
			return;
		}

		const context = canvas.getContext('2d');
		drawRect(context);
		if (exploreMode) {
			drawCleanup = tm_explore(context, machine, initial_tape, nbIter/*, headStyle*/);
		} else {
			tm_trace_to_image(
				context,
				machine,
				initial_tape,
				tapeWidth,
				nbIter,
				origin_x,
				true,
				headStyle
			);
		}
	}

	$: {
		// Dependencies:
		exploreMode;
		machine;
		initial_tape;
		tapeWidth;
		nbIter;
		origin_x;
		headStyle;

		if (canvas) {
			draw();
		}
	}
</script>

<div class="relative mr-5">
	<canvas class="bg-slate-800 image-render-pixel" bind:this={canvas} width="400" height="500" />
</div>
