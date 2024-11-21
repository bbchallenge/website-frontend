<script lang="ts">
	import { page } from '$app/stores';
	import { API } from '$lib/api_server';
	import { onMount } from 'svelte';
	import { HeadStyle, tm_trace_to_image, machineCodeToTM } from '$lib/tm';
	import '$lib/styles/tailwind.css';

	let generalisedIDAndParams = $page.params.gid;

	const generalisedID = generalisedIDAndParams.split('&')[0];

	let machineID = null;
	let machineCode = null;

	const nbIterDefault = 10000;
	const tapeWidthDefault = 400;
	const origin_xDefault = 0.5;
	let nbIter = nbIterDefault;
	let tapeWidth = tapeWidthDefault;
	let origin_x = origin_xDefault;

	const urlParams = new URLSearchParams(generalisedIDAndParams);
	if (urlParams.get('s') != null) {
		nbIter = Number(urlParams.get('s'));
	}
	if (urlParams.get('w') != null) {
		tapeWidth = Number(urlParams.get('w'));
	}
	if (urlParams.get('ox') != null) {
		origin_x = Number(urlParams.get('ox'));
	}

	if ((generalisedID.length > 0 && generalisedID[1] == 'R') || generalisedID[1] == 'L') {
		machineCode = generalisedID;
	} else {
		machineID = generalisedID;
	}

	let canvas;

	const drawRect = (context) => {
		context.fillStyle = 'black';
		context.fillRect(0, 0, canvas.width, canvas.height);
		context.fill();
	};

	let initial_tape = '0';
	// Default the params if called with null
	if (nbIter == null) {
		nbIter = nbIterDefault;
	}
	if (tapeWidth == null) {
		tapeWidth = tapeWidthDefault;
	}
	if (origin_x == null) {
		origin_x = origin_xDefault;
	}

	let machine = null;

	async function loadMachineFromID(localMachineID) {
		try {
			const response = await API.get(`/machine/${localMachineID}`, {});

			return machineCodeToTM(response.data['machine_code']);
		} catch (error) {
			console.log(error);
		}
	}

	function loadMachineFromMachineCode(machine_code) {
		machine_code = machine_code.trim().toUpperCase();
		try {
			return machineCodeToTM(machine_code);
		} catch (error) {}
	}

	let drawCleanup;
	function draw() {
		if (drawCleanup) drawCleanup();

		const context = canvas.getContext('2d');
		drawRect(context);

		tm_trace_to_image(
			context,
			machine,
			initial_tape,
			tapeWidth,
			nbIter,
			origin_x,
			true,
			HeadStyle.MOVEMENT
		);
	}

	onMount(async () => {
		try {
			if (machineID) {
				machine = await loadMachineFromID(machineID);
			} else {
				machine = loadMachineFromMachineCode(machineCode);
			}
			console.log(machine);
		} catch (error) {}

		draw();
		//console.log(metrics);
	});
</script>

<canvas class="bg-black image-render-pixel" bind:this={canvas} width="400" height="500" />
