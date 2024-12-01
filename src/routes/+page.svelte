<script>
  let shadowX = 8;
  let shadowY = 8;
  let shadowLength = 0;
  let cornerRadius = 8;
  let borderWidth = 2;
  let shadowColor = "#000";
  let elementColor = "#fff";
  let transitionLength = 0.2;

  let boxShadow = "";
  for (let i = 1; i < Math.max(shadowX, shadowY); i++) {
    boxShadow += `${shadowX / Math.max(shadowX, shadowY) * i}px ${shadowY / Math.max(shadowX, shadowY) * i}px 0px ${shadowLength}px ${shadowColor}, `;
  }
  boxShadow += `${shadowX}px ${shadowY}px ${shadowLength}px ${shadowColor}`;
</script>

<main>
  <div id="content"
  style="
  --shadow-x: {shadowX}px;
  --shadow-y: {shadowY}px;
  --shadow-length: {shadowLength}px;
  --corner-radius: {cornerRadius}px;
  --border-width: {borderWidth}px;
  --shadow-color: {shadowColor};
  --element-color: {elementColor};
  --transition-length: {transitionLength}s;
  --box-shadow: {boxShadow};
  ">
    <div id="settings">
      <h1>Neubrutalist UI Generator</h1>
    </div>

    <div class="component-card">
      <header>
        <h1>Button</h1>
      </header>
      <div class="component-wrapper">
        <button class="component">Click</button>
      </div>
    </div>
    <div class="code">

    </div>

    <div class="component-card">
      <header>
        <h1>Input</h1>
      </header>
      <div class="component-wrapper">
        <input type="text" class="component">
      </div>
    </div>
    <div class="code">

    </div>
  </div>
</main>

<style>
  #content {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: repeat(4, 300px);
    gap: 1rem;
    padding: 1rem;
  }

  #settings {
    grid-column: 1;
    grid-row: 1 / -1;
    display: flex;
    flex-direction: column;

    position: sticky;
    top: 0;
    left: 0;
    align-self: flex-start;

    gap: 1rem;
  }

  .component-card {
    gap: 1rem;
    border-radius: 1rem;
    background-color: var(--white);
    color: var(--black);
    display: grid;
    grid-template-rows: auto 1fr;
    padding: 1rem 1rem 3rem 1rem;
  }

  .component-card header {
    width: 100%;
  }

  .component-card .component-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .component {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 1rem;
    padding: 1rem 2rem;
    font-family: "Inter", sans-serif;
    font-size: 1rem;
    border-radius: var(--corner-radius);
    border: var(--border-width) solid var(--shadow-color);
    box-shadow: var(--box-shadow);
    background-color: var(--element-color);
    transition-property: transform, box-shadow;
    transition-duration: var(--transition-length);
  }

  input.component {
    padding: 1rem;
  }

  .component:active, input.component:focus {
    transform: translate(var(--shadow-x), var(--shadow-y));
    box-shadow: none;
    outline: none;
  }
</style>