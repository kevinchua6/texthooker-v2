<script lang="ts">
  import { onMount } from "svelte";
  let lines = 0;
  let chars = 0;

  function updateCounter() {
    const paragraphs = document.querySelectorAll("p");
    lines = paragraphs.length;
    chars = Array.from(paragraphs).reduce(
      (total, p) => total + p?.textContent.trim().length,
      0
    );
  }

  async function handleClipboardEvent() {
    try {
      const clipboardText = await navigator.clipboard.readText();
      const newParagraph = document.createElement("p");
      newParagraph.textContent = clipboardText;
      const body = document.querySelector("body");
      if (body?.lastChild?.textContent !== clipboardText) {
        document.querySelector("body")?.appendChild(newParagraph);
        updateCounter();
      }
    } catch (err) {
      console.log("huh");
      console.error("Failed to read clipboard contents: ", err);
    }
  }

  // Call updateCounter on mount to set initial counts
  onMount(() => {
    setInterval(handleClipboardEvent, 300);
    return () => {};
  });
</script>

<div class="container">
  <!-- This is the div used for the "remove last line" button. -->
  <button id="remove_button" title="Remove last line (X)">x</button>
  <!-- End button div. -->
  <!-- This is the div used for the counter. -->
  <div id="counter" title="No. of characters / No. of lines">
    Chars: {chars} / Lines: {lines}
  </div>
  <!-- End counter div. -->
</div>

<style>
  body {
    background-color: #bababa;
    color: #000000;
    height: 50000px;
  }

  .container {
    position: fixed;
    top: 3px;
    right: 5px;
    display: inline-block;
  }

  #remove_button {
    background-color: rgba(25, 25, 25, 0.8);
    color: #9d9d9d;
    font-size: 0.4em;
    line-height: 100%;
    float: right;
    margin-right: 10px;
    padding-left: 8px;
    padding-right: 8px;
    padding-top: 5px;
    padding-bottom: 5px;
    cursor: pointer;
    cursor: hand;
  }
</style>
