<script>
  export let keys;
  export let selectedNote;

  let showHints = false;
  let error = false;
  let success = false;

  function toggleHints() {
    showHints = !showHints;
  }

  function matchNotes(key) {
    const { value, freq } = key;
    playSound(freq);
    if (!selectedNote) return;

    error = value !== selectedNote;
    success = !error;

    const idx = keys.findIndex(k => k.value === value);
    const newKey = keys[idx];
    newKey.class = `${newKey.class} ${error ? "error" : "success"}`;
    keys[idx] = newKey;
  }

  function playSound(freq) {
    const context = new AudioContext();
    const o = context.createOscillator();
    const g = context.createGain();
    o.connect(g);
    o.type = "sine";
    o.frequency.value = freq;
    g.connect(context.destination);
    o.start(0);
    o.stop(context.currentTime + 0.5);
  }
</script>

<style>
  p,
  button {
    font-family: Lato;
  }

  button {
    cursor: pointer;
  }

  .content {
    background: #333;
  }

  .keyboard {
    display: flex;
    flex-direction: row;
    position: relative;
    margin: 0 1em 1em 1em;
  }

  .keyboard p {
    display: none;
  }

  .keyboard.hints .blanche p {
    height: 100%;
    width: 100%;
    display: flex;
    align-items: end;
    padding: 0;
    margin: 0;
    text-align: center;
    justify-content: center;
  }

  .keyboard.hints .noire p {
    color: white;
    height: 100%;
    width: 100%;
    display: flex;
    align-items: end;
    padding: 0;
    margin: 0;
    text-align: center;
    justify-content: center;
  }

  .blanche {
    background: white;
    border: 1px solid #ddd;
    border-radius: 0 0 5px 5px;
    height: 150px;
    width: 60px;
    transition: all 250ms;
  }

  .blanche:hover {
    background: #eee;
    transition: all 500ms;
  }

  .noire {
    background: #333;
    border: 1px solid #ddd;
    border-radius: 0 0 5px 5px;
    height: 75px;
    width: 30px;
    transition: all 250ms;
    position: absolute;
    z-index: 99;
  }

  .one {
    left: 46px;
  }
  .two {
    left: 107px;
  }
  .three {
    left: 227px;
  }
  .four {
    left: 286px;
  }
  .five {
    left: 346px;
  }

  .noire:hover {
    background: black;
    transition: all 500ms;
  }

  .success {
    background: #4caf50;
  }

  .error {
    background: #f44336;
  }

  .success:hover {
    background: #66bb6a;
  }

  .error:hover {
    background: #ef5350;
  }
</style>

<div class="content">
  <div class="keyboard {showHints ? 'hints' : ''}">
    {#each keys as key, index (key)}
      <button class={key.class} on:click={() => matchNotes(key)}>
        <p>{key.value}</p>
      </button>
    {/each}
  </div>
</div>

<label>
  <input type="checkbox" on:click={toggleHints} />
  Show hints
</label>
