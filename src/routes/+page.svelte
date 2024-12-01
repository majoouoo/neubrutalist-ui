<script>
  let shadowX = 10;
  let shadowY = 10;
  let shadowLength = 0;
  let cornerRadius = 8;
  let borderWidth = 2;
  let shadowColor = "#000";
  let elementColor = "#fff";
  let transitionLength = 0.2;
  let fontSizeRem = 1;

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
  --font-size-rem: {fontSizeRem}rem;
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
        <button class="component">Log in</button>
      </div>
    </div>
    <div class="code">

    </div>

    <div class="component-card">
      <header>
        <h1>Input</h1>
      </header>
      <div class="component-wrapper">
        <div class="input-wrapper">
          <input type="text" name="Input" class="component" placeholder=" ">
          <label for="Input" class="input-label">Username</label>
        </div>
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
    padding: var(--font-size-rem) calc(var(--font-size-rem) * 2);
    font-family: "Inter", sans-serif;
    font-size: var(--font-size-rem);
    border-radius: var(--corner-radius);
    border: var(--border-width) solid var(--shadow-color);
    box-shadow: var(--box-shadow);
    background-color: var(--element-color);
    color: var(--shadow-color);
    transition-property: transform, box-shadow;
    transition-duration: var(--transition-length);
  }

  .component:active, input.component:focus {
    transform: translate(var(--shadow-x), var(--shadow-y));
    box-shadow: none;
    outline: none;
  }

  input.component {
    padding: var(--font-size-rem);
    position: relative;
    grid-row: 1;
    grid-column: 1;
  }

  .input-label {
    font-size: var(--font-size-rem);
    font-family: "Inter", sans-serif;
    grid-row: 1;
    grid-column: 1;
    z-index: 1;
    pointer-events: none;
    transform: translateY(var(--font-size-rem)) translateX(calc(var(--font-size-rem) / 2)) scale(1);
    background-color: white;
    width: min-content;
    height: min-content;
    padding: 0 calc(var(--font-size-rem) / 2);
    transition: transform var(--transition-length);
    color: var(--shadow-color);
  }

  .input-wrapper {
    display: grid;
    grid-template-columns: 100%;
    grid-template-rows: 100%;
  }

  input.component:not(:placeholder-shown) ~ .input-label {
    transform: translateY(calc(var(--font-size-rem) * -0.5)) translateX(0) scale(0.8);
  }

  input.component:focus ~ .input-label {
    transform: translateY(calc(var(--font-size-rem) * -0.5 + var(--shadow-y))) translateX(var(--shadow-x)) scale(0.8);
  }
</style>