<script>
  import { createEventDispatcher } from "svelte";

  export let keys;

  const notes = keys.map(k => k.value);
  const dispatch = createEventDispatcher();

  let selectedNote = null;
  let randIdx = null;
  let showNote = false;
  let showReplay = false;
  let showMusicSheet = false;

  function toggleHints() {
    showHints = !showHints;
  }

  function usingNotation() {
    randomNote();
    showNote = true;
    showReplay = false;
    showMusicSheet = false;
  }

  function usingSound() {
    randomNote();
    showNote = false;
    showReplay = true;
    showMusicSheet = false;
    playSound(keys[randIdx].freq);
  }

  function usingMusicSheet() {
    randomNote();
    showNote = false;
    showReplay = false;
    showMusicSheet = true;

    // TODO fetch the right png
  }

  function randomNote() {
    const randMin = 0;
    const randMax = notes.length;
    randIdx = Math.floor(Math.random() * (randMax - randMin)) + randMin;

    selectedNote = notes[randIdx];

    dispatch("select", { note: selectedNote });
  }

  function playSound() {
    console.log("ici", keys[randIdx].freq);
    const context = new AudioContext();
    const o = context.createOscillator();
    const g = context.createGain();
    o.connect(g);
    o.type = "sine";
    o.frequency.value = keys[randIdx].freq;
    g.connect(context.destination);
    o.start(0);
    o.stop(context.currentTime + 0.5);
  }
</script>

<style>
  div {
    display: flex;
    align-items: baseline;
  }

  button {
    margin-right: 1em;
  }

  p,
  button {
    font-family: Lato;
  }
</style>

<p>Pick a note</p>
<div>
  <button on:click={usingNotation}>Using notation</button>
  <button on:click={usingSound}>Using sound</button>
  <button on:click={usingMusicSheet}>Using music sheet</button>
</div>

{#if showNote}
  <p>
    Find this note:
    <b>{selectedNote}</b>
  </p>
{/if}

{#if showReplay}
  <button on:click={playSound}>Listen again</button>
{/if}

{#if showMusicSheet}TODO{/if}
