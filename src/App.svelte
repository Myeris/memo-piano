<script>
  import Piano from "./Piano.svelte";
  import NotePicker from "./NotePicker.svelte";

  const initialKeys = [
    { value: "C", class: "blanche", freq: 261.63 },
    { value: "C#", class: "noire one", freq: 277.18 },
    { value: "D", class: "blanche", freq: 293.66 },
    { value: "D#", class: "noire two", freq: 311.13 },
    { value: "E", class: "blanche", freq: 329.63 },
    { value: "F", class: "blanche", freq: 349.23 },
    { value: "F#", class: "noire three", freq: 369.99 },
    { value: "G", class: "blanche", freq: 392.0 },
    { value: "G#", class: "noire four", freq: 415.3 },
    { value: "A", class: "blanche", freq: 440 },
    { value: "A#", class: "noire five", freq: 466.16 },
    { value: "B", class: "blanche", freq: 493.88 }
  ];
  let keys = initialKeys.map(k => Object.assign({}, k));
  let selectedNote;

  function resetKeysState() {
    keys = initialKeys.map(k => Object.assign({}, k));
  }

  function setSelectedNote(value) {
    selectedNote = value.detail.note;
    resetKeysState();
  }
</script>

<style>
  #root {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>

<div id="root">
  <h1>MemoPiano</h1>
  <NotePicker {keys} on:select={v => setSelectedNote(v)} />
  <Piano {keys} {selectedNote} />

  <div>
    The keyboard above goes from
    <b>C4</b>
    to
    <b>B4</b>
    .
  </div>
</div>
