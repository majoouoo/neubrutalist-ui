<script>
import { onMount } from "svelte";
import { createHighlighter } from "shiki";

export let language = "";
export let code = "";

let highlighter;
let codeHtml;

onMount(async () => {
  highlighter = await createHighlighter({
    themes: ['github-dark'],
    langs: ['html', 'css'],
  })

  generateHtml(code)
})

const generateHtml = (code) => {
  if (highlighter) {
    codeHtml = highlighter.codeToHtml(code, {
      lang: language,
      theme: "github-dark",
    });
  }
}

$: generateHtml(code);

const copy = () => {
  navigator.clipboard.writeText(code);
}

</script>

<code>
  {@html codeHtml}
  
  <button class="copyBtn" on:click={copy}>
    Copy
  </button>
</code>

<style>
  code {
    border-radius: 0.5rem;
    overflow-y: scroll;
    width: 100%;
    height: 100%;
    background-color: #24292e;
    padding: 0.5rem;
    font-family: monospace;
    box-sizing: border-box;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  .copyBtn {
    background-color: #24292e;
    color: white;
    border: 1px solid #424b55;
    padding: 0.5rem;
    border-radius: 0.5rem;
    cursor: pointer;
    position: sticky;
    bottom: 0;
    left: 0;
    transition: background-color 0.2s;
  }

  .copyBtn:hover {
    background-color: #424b55;
  }

  .copyBtn:active {
    background-color: #5c6977;
    border: 1px solid #5c6977;
  }
</style>