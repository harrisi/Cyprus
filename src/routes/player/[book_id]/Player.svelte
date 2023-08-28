<script>
  // export let id;
  export let title;
  export let author;
  export let cover;
  export let files;

  let audio = new Audio(files[0]);
  let isPlaying = false;
  let timeNow = 0;
  let timeMax = 0;
  audio.addEventListener('timeupdate', () => timeNow = audio.currentTime)
  audio.addEventListener('loadeddata', () => timeMax = audio.duration, { once: true })
  audio.addEventListener('play', () => isPlaying = true)
  audio.addEventListener('pause', () => isPlaying = false)

  function parse(sec) {
    const hours = Math.floor(sec / 60 / 60).toString().padStart(2, '0')
    const minutes = Math.floor(sec / 60).toString().padStart(2, '0')
    const seconds = Math.round(sec % 60).toString().padStart(2, '0')

    return `${sec > 3600 ? hours + ':' : ''}${minutes}:${seconds}`
  }
</script>

<div class="player">
  <img src={cover} alt="Cover Art" />
  <p>{title} by {author}</p>
  <p>{parse(timeNow)} / {parse(timeMax)}</p>
  <input type="range" min=0 max={timeMax} bind:value={audio.currentTime} />
  <div class="controls">
    <button on:click={() => audio.currentTime -= 15} class="control">
      <img class="rewind" src='/icons/skip.png' alt="Rewind button" />
    </button>
    <button class="control" on:click={() => audio.paused ? audio.play() : audio.pause()}>
      <img src={`/icons/${isPlaying ? 'pause' : 'play'}.png`} alt="Play/Pause button" />
    </button>
    <button on:click={() => audio.currentTime += 15} class="control">
      <img src='/icons/skip.png' alt="Fast forward button" />
    </button>            
  </div>
</div>

<style>
  .player {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
  }

  .controls {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
  }

  .control {
    padding: 10px;
  }

  button {
    background-color: transparent;
    border: none;
    padding: 0;
    transform: translate(0, -8px);
    transition: all 0.1s;
  }

  img.rewind {
    transform: scaleX(-1);
  }

  button:active {
    transform: translate(0, -2px);
  }
</style>
