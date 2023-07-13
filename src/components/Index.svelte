<script>
	import { getContext, onMount } from "svelte";
	import Demo from "$components/demo/Demo.svelte";
	import WIP from "$components/helpers/WIP.svelte";
	// import Footer from "$components/Footer.svelte";
	import * as Tone from 'tone'
	import { range, scaleLinear } from "d3";

	let mounted;

	let player;
	let buffer
	let keys;
	let sampler;
	let vol;

	let keyClicked = { "xyz": "clicked"};

	import Keyboard from "svelte-keyboard";

	const onKeyUp = (e) => {
		keyClicked = {};
	}


	const onKeydownTwo = (event) => {
	console.log(event.detail);
	}


	const onKeyDown = (e) => {
		keyClicked = {};
		keyClicked[e.key.toLowerCase()] = "clicked";
		// keys.player((e.keyCode % 10)).start(Tone.context.currentTime,e.keyCode % 10)

		console.log(e.keyCode % 12,e.keyCode)
		keys.player(e.keyCode % 12).start(Tone.context.currentTime,0)
		// player.start()
		if(e.keyCode == 65){

			
			// player.start(Tone.context.currentTime);
		}
		else {
			// const play = new Tone.Player().toDestination().connect(vol);
			// play.buffer = buffer.slice(0,.18);
			// // play.buffer = buffer.slice(e.keyCode % 20,e.keyCode % 20+2);
			// play.start(Tone.context.currentTime)

		}
		// else {
			// const play = new Tone.Player().toDestination();
		// }
		// sampler.triggerAttack(["A1"], .5);
		

		// keys.player((e.keyCode % 10)).start(0,e.keyCode % 10,2)

	}

	const playSynth = () => {
		// player.start(Tone.context.currentTime)

		// player.buffer = buffer.slice(5,7);
		// player.start(0,0,2)

		// player("C4").start(0,0,"16t");
		// player.start(0,1);
      	// player.stop(3);

		// keys.player(1).start(0,10*Math.random(),2)
		// keys.player(0).start(0,50*Math.random(),2)


		// const playerSingle = new Tone.Player({
		// 	url:"assets/audio.mp3",
		// 	loop:false,
		// 	loopStart:5,
		// 	loopEnd:8,
		// }).toDestination();

		// Tone.loaded().then(() => {
		// 	playerSingle.start(0,5,6);
		// })



		// player.start(0,5,8);
		// player.start("8t",4,9);
		
		// const now = Tone.now()
		// for (let i = 1; i < 4; i++){
		// 	player.start("16t", i+5, i+5+2)
    	// }
	}

	const playStop = () => {
		player.stop();
	}
	
	onMount(() => {
		// player = new Tone.Player({
		// 	url:"assets/audio.mp3",
		// 	loop:false,
		// 	loopStart:5,
		// 	loopEnd:8,
		// }).toDestination();



		// console.log(buffers)


		// keys = new Tone.Players({
		// 	urls: buffers,
		// 	baseUrl: "assets/",
		// }).toDestination();
	
		Tone.start();
	    Tone.context.lookAhead = 0;

		Tone.Transport.bpm.value = 115;
		let fourth = Tone.Time("4n").toSeconds()*2;
		console.log(fourth)

		vol = new Tone.Volume(-100).toDestination();

		buffer = new Tone.ToneAudioBuffer("assets/trimmed.m4a", (e) => {

			let counts = range(12);
			let buffers = {};
			
			counts.forEach((d) => {
				console.log(d*fourth,d*fourth+fourth)
				buffers[d] = buffer.slice(d*fourth,d*fourth+fourth);
			})

			console.log(buffers)

			keys = new Tone.Players({
				urls: buffers
			}).toDestination();


			
			// player = new Tone.Player().toDestination();
			// player.buffer = buffer//.slice(0,12)//.slice(0,12);
			// sampler = new Tone.Sampler({
			// 	urls: {
			// 		A1: "A1.mp3",
			// 		A2: "A2.mp3",
			// 	},
			// 	baseUrl: "https://tonejs.github.io/audio/casio/",
			// }).toDestination();
		});




		Tone.loaded().then(() => {
			mounted = true;
		});
	})
	// const copy = getContext("copy");
	// const data = getContext("data");

</script>

<div class="keyboard">
{#if buffer && mounted}
	<Keyboard keyClass="{keyClicked}" on:keydown="{onKeydownTwo}" />
	<!-- <div class="" role="group" aria-label="Keyboard"
		<div class="row">
		  <button type="button" data-key="q" class="">
			q
		  </button>
		  <button type="button" data-key="w" class="">
			w
		  </button>
		  <button type="button" data-key="e" class="">
			e
		  </button>
		  <button type="button" data-key="r" class="">
			r
		  </button>
		  <button type="button" data-key="t" class="">
			t
		  </button>
		  <button type="button" data-key="y" class="">
			y
		  </button>
		  <button type="button" data-key="u" class="">
			u
		  </button>
		  <button type="button" data-key="i" class="">
			i
		  </button>
		  <button type="button" data-key="o" class="">
			o
		  </button>
		  <button type="button" data-key="p" class="">
			p
		  </button>
		</div>
		<div class="row">
		  <div data-testid="spacer" class=""></div>
		  <button type="button" data-key="a" class="">
			a
		  </button>
		  <button type="button" data-key="s" class="">
			s
		  </button>
		  <button type="button" data-key="d" class="">
			d
		  </button>
		  <button type="button" data-key="f" class="">
			f
		  </button>
		  <button type="button" data-key="g" class="">
			g
		  </button>
		  <button type="button" data-key="h" class="">
			h
		  </button>
		  <button type="button" data-key="j" class="">
			j
		  </button>
		  <button type="button" data-key="k" class="">
			k
		  </button>
		  <button type="button" data-key="l" class="">
			l
		  </button>
		  <div data-testid="spacer" class=""></div>
		</div>
		<div class="row">
		  <button
			type="button"
			data-key="↵"
			class=" "
		  >
			enter
		  </button>
		  <button type="button" data-key="z" class="">
			z
		  </button>
		  <button type="button" data-key="x" class="">
			x
		  </button>
		  <button type="button" data-key="c" class="">
			c
		  </button>
		  <button type="button" data-key="v" class="">
			v
		  </button>
		  <button type="button" data-key="b" class="">
			b
		  </button>
		  <button type="button" data-key="n" class="">
			n
		  </button>
		  <button type="button" data-key="m" class="">
			m
		  </button>
		  <button
			type="button"
			data-key="←"
			aria-label="backspace"
			class=" "
		  >
			<svg
			  xmlns="http://www.w3.org/2000/svg"
			  height="20"
			  viewBox="0 0 24 24"
			  width="20"
			  class="game-icon"
			  data-testid="icon-backspace"
			>
			  <path
				fill="var(--color-tone-1)"
				d="M22 3H7c-.69 0-1.23.35-1.59.88L0 12l5.41 8.11c.36.53.9.89 1.59.89h15c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm0 16H7.07L2.4 12l4.66-7H22v14zm-11.59-2L14 13.41 17.59 17 19 15.59 15.41 12 19 8.41 17.59 7 14 10.59 10.41 7 9 8.41 12.59 12 9 15.59z"
			  ></path>
			</svg>
		  </button>
		</div>
	  </div>
	  > -->
	  

{/if}
</div>
<svelte:window on:keydown|preventDefault={onKeyDown} on:keyup|preventDefault={onKeyUp} />


<!-- <WIP />
<Demo /> -->
<!-- <Footer /> -->


<style>
	.keyboard {
		margin-top: 100px;
	}

	.row {
		display: flex;
		width: 100%;
		margin: 0 auto 8px;
		touch-action: manipulation;
	}
	.row button {
		font-size: 1.25em;
		font-weight: bold;
		border: 0;
		padding: 0;
		margin: 0 6px 0 0;
		height: 58px;
		color: black;
		border-radius: 4px;
		cursor: pointer;
		-webkit-user-select: none;
		-moz-user-select: none;
		user-select: none;
		flex: 1;
		display: flex;
		justify-content: center;
		align-items: center;
		text-transform: uppercase;
		-webkit-tap-highlight-color: rgba(0,0,0,.3);
	}
</style>