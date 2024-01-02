<!-- src/App.svelte -->
<script>
	import { writable } from 'svelte/store';
  
	let hours = 16;
	let minutes = 0;
	let seconds = 0;
	let interval;
	let paused = false;
	// let sound = new Audio('path/to/your/soundfile.mp3');
	let sound = new Audio('alarm-clock-short-6402.mp3');
  
	const time = writable(formatTime());
  
	function formatTime() {
	  return `${hours}h ${minutes}m ${seconds}s`;
	}
  
	function init() {
	  interval = setInterval(() => {
		if (!paused) {
		  if (seconds > 0) {
			seconds--;
		  } else if (minutes > 0) {
			minutes--;
			seconds = 59;
		  } else if (hours > 0) {
			hours--;
			minutes = 59;
			seconds = 59;
		  } else {
			clearInterval(interval);
			playSound();
		  }
		  time.set(formatTime());
		}
	  }, 1000);
	}
  
	function togglePauseResume() {
	  paused = !paused;
	}
  
	function reset() {
	  clearInterval(interval);
	  paused = false;
	//   hours = 16;
	//   minutes = 0;
	  seconds = 0;
	  time.set(formatTime());
	}
  
	function start() {
	  reset();
	  init();
	}
  
	function selectDuration(hoursArg, minutesArg=0) {
	  reset();
	  hours = hoursArg;
	  minutes = minutesArg
	  
	  time.set(formatTime());
	}

	function playSound() {
    	sound.play();
  	}
  </script>
  
  <style>
	/* Add your styles here if needed */
  </style>
  
  <div>
	<h1>Countdown Timer</h1>
	<p>{$time}</p>
	<button on:click={togglePauseResume}>{paused ? 'Resume' : 'Pause'}</button>
	<button on:click={reset}>Reset</button>
	<button on:click={start}>Start</button>
	<hr/>

	<h2>Pomodo versions</h2>
	<button on:click={() => selectDuration(0, 25)}>25 Minutes</button>
	<button on:click={() => selectDuration(0, 15)}>15 Minutes</button>
	<button on:click={() => selectDuration(0, 5)}>5 Minutes</button>
	<button on:click={() => selectDuration(0, 1)}>1 Minutes</button>

	<hr/>

	<h2>Daily Targets</h2>
	<button on:click={() => selectDuration(16)}>16 Hours</button>
	<button on:click={() => selectDuration(8,0)}>8 Hours</button>
	<button on:click={() => selectDuration(4,0)}>4 Hours</button>
	<button on:click={() => selectDuration(2,0)}>2 Hours</button>
	<button on:click={() => selectDuration(1,0)}>1 Hour</button>

	
  </div>
  