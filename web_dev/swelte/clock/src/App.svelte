<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:ital,wght@1,300&family=Roboto+Mono:wght@400;700&display=swap" rel="stylesheet">

<!--<link href='https://fonts.googleapis.com/css?family=Orbitron' rel='stylesheet' type='text/css'> -->
<script>
	import { Clock } from "./clock.js";
	import {fly, fade } from "svelte/transition";
	import { spring } from 'svelte/motion';
	
	let clock = new Clock(23,37);
	let alarm;	

	let alarmNotDefined = false;

	let minuteClock = spring(parseInt(clock.time.hour) * 60 + parseInt(clock.time.minute))

	function tick() {
        clock.tick();
		minuteClock.set( parseInt(clock.time.hour) * 60 + parseInt(clock.time.minute))
        clock = clock;
    }

	setInterval(tick,1000)

	function activateAlarm() {
		if(alarm) {
			clock.alarm = alarm
			alarmNotDefined = false
		} else {
			alarmNotDefined = true
		}
	}

</script>

<style>
	:global(body){
		background-size: 400% 400% !important;
		background-color: rgb(215,255,0);
background: linear-gradient(45deg, rgba(215,255,0,0.9475140397956058) 12%, rgb(151, 177, 33) 25%, rgba(87,255,0,1) 45%, rgba(255, 197, 38, 0.76) 71%, rgb(50,234,62) 90%);
		animation: gradient 30s ease infinite;
		font-family: 'Roboto Mono', monospace;		
	}
	@keyframes gradient {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;		
	}
	.container{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		width: 100%;
		align-items: stretch;
		justify-content: space-around;
		align-content: center;
		text-align: center;
		flex-wrap: wrap;
    }
        .clock{
            width: 30%;
			min-width: 200px;
            background-color: rgb(47,79,79);
            border: black solid 5px;
            margin: 5px;
			border-radius: 50px;
			padding: 10px 0 10px 0;
			color: whitesmoke;
        }

		.clock:active{
			background-color: rgba(0, 0, 0, 1);
			border-color: rgb(89, 152, 152);
		}

		.clock:hover{
			animation: changeBackground 1s 1;
			background-color: black;
			border-color: rgb(89, 152, 152);
		}

		@keyframes changeBackground {
			from {background-color: rgb(47,79,79)}
  			to {background-color: rgb(0, 0, 0)}
		}

		b {
			color: rgb(243, 83, 91);
		}

		.time{
			font-size: 3em;
			margin: 25px 0;
			font-family: 'IBM Plex Mono','Orbitron', 'sans-serif';
			font-style: italic;
			font-weight: bold;
			letter-spacing: 0.1em;
		}

	#digitalClock{
		position: absolute;
		margin: 0 auto;
		width: 200px;
		height: 72.8px;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}

	svg {
		width: 100%;
	}

	.clock-face {
		stroke: #333;
		fill: white;
	}

	.minor {
		stroke: #999;
		stroke-width: 0.5;
	}

	.major {
		stroke: #333;
		stroke-width: 1;
		
	}

	.hour {
		stroke: #333;
	}

	.minute {
		stroke: #666;
	}
	
	@media (max-width: 1000px) {
		.container {
			flex-direction: column;	
		}

		.clock {
			width: 70%;
			margin: 5px auto;
		}
	}

	.alarmclock {
		margin: 0;
	}

	button, input {
		background-color: rgb(162, 205, 220);
		cursor: pointer;
		border-radius: 5px;
		padding: 10px 15px;
		text-align: center;
	}

	button.setAlarm:hover, 
	input:hover {
		animation: buttonChangeBackground 0.25s 1;
		background-color: rgb(129, 129, 130);
		color: white;
	}

	button.setAlarm:active, 
	input:active {
		color: white;
		background-color: rgb(129, 129, 130);
	}

	button.disableAlarm {
		color: white; 
		background: rgba(255, 0, 0, 0.75);
	}

	button.disableAlarm:hover, 
	button.disableAlarm:active {
		background-color: red;
	}

	@keyframes buttonChangeBackground{
		from {background-color: rgb(162, 205, 220)}
  		to {background-color: rgb(129, 129, 130)}
	}

	.tttt{
		font-weight: bolder;
		fill: white;
		font-size: 2em;
	}

	.alarmWakeUp{
		height: 80px;
		position: relative;
		font-size: 2.5em;
		color: rgb(170, 19, 19);
		display: inline-block;
		animation: moveAround 1s ease infinite;
	}

	@keyframes moveAround{
		0% {
			margin-top: 5px;
		}

		50% {
			margin-top: 100px;
		}

		100% {
			margin-top: 5px;
			color:rgb(255, 0, 0);
		}
	}

	@media (max-width: 500px) {
		.clock{
			width: 100%;
			margin: 5px auto;
		}

		.alarmWakeUp{
			font-size: 2em;
		}
	}
	@media (max-width: 300px) {
		p {
			font-size: 15px;
		}
		input, 
		button {
			padding: 10px 7.5px;
		}
		
		.alarmWakeUp{
			font-size: 1em;
		}
	}

</style>

<main>
	<h1>Clock Viewer</h1>
	{#if clock.isTriggered}
			<div style="height: 200px;">	
				<div class="alarmWakeUp">
					<h2 class="alarmclock">Wake up!!</h2>
				</div>
			</div>
	{/if}
	<div class="container">
		<div class="clock">
					<div style="position: relative; height:200px;">
						<div id="digitalClock">
							{#key clock.time.hour}
								<span style="position: absolute; margin:0; top: 0px; left: 0;" in:fly="{{ y: -25}}" class= "time">{clock.time.hour.toString().padStart(2, "0")}</span>
							{/key}	
								<span style="font-size: 3em; position: absolute; margin: 0; top: -6px; left: 80px; display: inline-block; height: 5px">:</span>
							{#key clock.time.minute}
								<span style="position: absolute; margin: 0; top: 0; right: 5px; " in:fly="{{ y: -25}}" class= "time">{clock.time.minute.toString().padStart(2, "0")}</span>
							{/key}
						</div>
					</div>
					<div class="item" style="margin-top: 25px;">
						<input bind:value={alarm} type="time">			
						{#if clock._alarmIsActive}
							<button class="disableAlarm" on:click={()=>{ clock.deactivateAlarm(); alarm = ""; }}>Disable</button>
						{:else if !clock._alarmIsActive}
							<button class="setAlarm" on:click={activateAlarm}>Set Alarm</button>
						{/if}

						{#if clock._alarmIsActive && !clock.isTriggered}
							<p in:fade>Alarm set to: <b>{clock.alarm}</b></p>
						{:else if (alarmNotDefined && !clock.isTriggered)}
							<p in:fade>Set alarm to a valid time</p>
						{/if}
					</div>
		</div>	
		<div class="clock">
			<!-- Inspired by: https://svelte.dev/examples#clock -->
				<div class="item" style="height: 200px;">
					<svg viewBox='-50 -50 100 100' style="width: 70%; height:100%">
						<circle class='clock-face' r='48'/>
						<!-- markers -->
						{#each [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55] as minute}
							<line
								class='major'
								y1='35'
								y2='45'
								transform='rotate({30 * minute})'
							/>
							{#each [1, 2, 3, 4] as offset}
								<line
									class='minor'
									y1='42'
									y2='45'
									transform='rotate({6 * (minute + offset)})'
								/>
							{/each}
						{/each}
						<!-- hour hand -->
						<line
							class='hour'
							y1='2'
							y2='-20'
							transform='rotate({0.5 * $minuteClock})'
						/>
					
						<!-- minute hand -->
						<line
							class='minute'
							y1='4'
							y2='-30'
							transform='rotate({6 * $minuteClock})'
						/>
					</svg>
					</div>
				<div style="margin-top: 25px;">
					<input bind:value={alarm} type="time">
					{#if clock._alarmIsActive}
						<button class="disableAlarm" on:click={()=>{ clock.deactivateAlarm(); alarm = ""; }}>Disable</button>
					{:else if !clock._alarmIsActive}
						<button class="setAlarm" on:click={activateAlarm}>Set Alarm</button>
					{/if}

				{#if clock._alarmIsActive && !clock.isTriggered}
					<p in:fade>Alarm set to: <b>{clock.alarm}</b></p>
					{:else if (alarmNotDefined && !clock.isTriggered)}
						<p in:fade>Set alarm to a valid time</p>
				{/if}
				</div>
		</div>
		<div class="clock">
			<div style="height: auto; display:flex; justify-content: space-around; text-align: center; margin-top: 10px; margin-left: 10px; margin-right: 10px;">
				<div class="hourMeter" style="width: 50%; height: 100%">
					<svg width="80" height="200" style="width: 80%; border: 3px solid green;">
						<rect x= "0" y= "{198 - clock.time.hour * 8.53}" width="100%" height="200" style="fill:rgb(0,0,255);stroke:rgb(0,0,0)" />
						<text class="tttt" x="50%" y="50%" dominant-baseline="middle" text-anchor="middle">{clock.time.hour.toString().padStart(2, "0")}</text>
					</svg> 
				</div>
				<div class="minuteMeter" style="width: 50%; height: 100%">
					<svg width="80" height="200" style="width: 80%; border: 3px solid green;">
						<rect x= "0" y= "{198 - clock.time.minute*3.33}" width="100%" height="200" style="fill:rgb(0,0,255);stroke:rgb(0,0,0);" />
						<text class="tttt" x="50%" y="50%" dominant-baseline="middle" text-anchor="middle">{clock.time.minute.toString().padStart(2, "0")}</text>	
				</div>					
			</div> 
			<div style="margin-top: 5px;">
				<input bind:value={alarm} type="time">
			{#if clock._alarmIsActive}
				<button class="disableAlarm" on:click={()=>{ clock.deactivateAlarm(); alarm = ""; }}>Disable</button>
			{:else if !clock._alarmIsActive}
				<button class="setAlarm" on:click={activateAlarm}>Set Alarm</button>
			{/if}

			{#if clock._alarmIsActive && !clock.isTriggered}
				<p in:fade>Alarm set to: <b>{clock.alarm}</b></p>
			{:else if (alarmNotDefined && !clock.isTriggered)}
				<p in:fade>Set alarm to a valid time</p>
			{/if}
			</div>
		</div>
	</div>
</main>