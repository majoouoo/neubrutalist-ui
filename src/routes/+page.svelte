<script>
	import CodeBlock from '$lib/CodeBlock.svelte';
	import { onMount } from 'svelte';
	import { createHighlighter } from 'shiki';

	let highlighter;
	onMount(async () => {
		highlighter = await createHighlighter({
			themes: ['github-dark'],
			langs: ['html', 'css']
		});
	});

	let shadowX = 10;
	let shadowY = 10;
	let cornerRadius = 8;
	let borderWidth = 2;
	let shadowColor = '#000000';
	let elementColor = '#ffffff';
	let checkedColor = '#009d30';
	let backgroundColor = '#ffffff';
	let transitionDuration = 0.2;
	let fontSize = 16;

	$: shadowX = String(shadowX).replace(/[^0-9.-]/g, '');
	$: shadowY = String(shadowY).replace(/[^0-9.-]/g, '');
	$: cornerRadius = String(cornerRadius)
		.replace(/-\d+(\.\d+)?/g, '0')
		.replace(/[^0-9.]/g, '');
	$: borderWidth = String(borderWidth)
		.replace(/-\d+(\.\d+)?/g, '0')
		.replace(/[^0-9.]/g, '');
	$: transitionDuration = String(transitionDuration)
		.replace(/-\d+(\.\d+)?/g, '0')
		.replace(/[^0-9.]/g, '');
	$: fontSize = String(fontSize)
		.replace(/-\d+(\.\d+)?/g, '0')
		.replace(/[^0-9.]/g, '');

	// Shadow calculation
	let boxShadow = '';
	const updateBoxShadow = (shadowX, shadowY, shadowColor) => {
		boxShadow = '';
		for (let i = 1; i < Math.max(Math.abs(shadowX), Math.abs(shadowY)); i++) {
			boxShadow += `${(shadowX / Math.max(Math.abs(shadowX), Math.abs(shadowY))) * i}px ${(shadowY / Math.max(Math.abs(shadowX), Math.abs(shadowY))) * i}px ${shadowColor}, `;
		}
		boxShadow += `${shadowX}px ${shadowY}px ${shadowColor}`;
	};
	$: updateBoxShadow(shadowX, shadowY, shadowColor);

	let switchShadow = '';
	const updateSwitchShadow = (shadowColor, fontSize) => {
		switchShadow = '';
		for (let i = 1; i < fontSize * 2 + 10; i += 3) {
			switchShadow += `0px ${i}px ${shadowColor}, `;
		}
		switchShadow += `0px ${fontSize * 2}px ${shadowColor}`;
	};
	$: updateSwitchShadow(shadowColor, fontSize);

	// Increment setting
	let incrementInterval;
	const startIncrement = (setting, increment) => {
		incrementSetting(setting, increment);
		incrementInterval = setInterval(() => incrementSetting(setting, increment), 130);
	};

	const stopIncrement = () => {
		clearInterval(incrementInterval);
	};

	const incrementSetting = (setting, increment) => {
		switch (setting) {
			case 'shadowX':
				shadowX = Number(shadowX) + increment;
				break;
			case 'shadowY':
				shadowY = Number(shadowY) + increment;
				break;
			case 'cornerRadius':
				cornerRadius = Number(cornerRadius) + increment;
				break;
			case 'borderWidth':
				borderWidth = Number(borderWidth) + increment;
				break;
			case 'transitionDuration':
				transitionDuration *= 10;
				transitionDuration = Number(transitionDuration) + increment;
				transitionDuration /= 10;
				break;
			case 'fontSize':
				fontSize = Number(fontSize) + increment;
				break;
		}
	};

	// Code tabs
	let activeCodeTabs = {
		button: 'html',
		input: 'html',
		checkbox: 'html',
		card: 'html'
	};
	const setActiveCodeTab = (component, tab) => {
		activeCodeTabs[component] = tab;
	};
</script>

<main>
	<div id="hero">
		<h1>Neubrutalist UI <span class="saint-font" id="heading-g">G</span>enerator</h1>
		<p>Effortlessly create bold, eye-catching interfaces with these fully customizable elements.</p>
	</div>
	<div
		id="content"
		style="
  --shadow-x: {shadowX}px;
  --shadow-y: {shadowY}px;
  --corner-radius: {cornerRadius}px;
  --border-width: {borderWidth}px;
  --shadow-color: {shadowColor};
  --element-color: {elementColor};
  --background-color: {backgroundColor};
  --checked-color: {checkedColor};
  --transition-duration: {transitionDuration}s;
  --font-size-rem: {fontSize / 16}rem;
  --box-shadow: {boxShadow};
  --switch-shadow: {switchShadow};
  "
	>
		<div id="settings">
			<h1>Settings</h1>
			<div class="setting">
				<label for="shadow-x">Shadow X (px):</label>
				<div class="setting-input-wrapper">
					<input type="text" name="shadow-x" bind:value={shadowX} />
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('shadowX', 1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						+</button
					>
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('shadowX', -1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						-</button
					>
				</div>
			</div>
			<div class="setting">
				<label for="shadow-y">Shadow Y (px):</label>
				<div class="setting-input-wrapper">
					<input type="text" name="shadow-y" bind:value={shadowY} />
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('shadowY', 1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						+</button
					>
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('shadowY', -1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						-</button
					>
				</div>
			</div>
			<div class="setting">
				<label for="corner-radius">Corner radius (px):</label>
				<div class="setting-input-wrapper">
					<input type="text" name="corner-radius" bind:value={cornerRadius} />
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('cornerRadius', 1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						+</button
					>
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('cornerRadius', -1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						-</button
					>
				</div>
			</div>
			<div class="setting">
				<label for="border-width">Border width (px):</label>
				<div class="setting-input-wrapper">
					<input type="text" name="border-width" bind:value={borderWidth} />
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('borderWidth', 1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						+</button
					>
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('borderWidth', -1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						-</button
					>
				</div>
			</div>
			<div class="setting">
				<label for="shadow-color">Shadow color:</label>
				<input type="color" name="shadow-color" bind:value={shadowColor} />
			</div>
			<div class="setting">
				<label for="element-color">Element color:</label>
				<input type="color" name="element-color" bind:value={elementColor} />
			</div>
			<div class="setting">
				<label for="background-color">Background color:</label>
				<input type="color" name="background-color" bind:value={backgroundColor} />
			</div>
			<div class="setting">
				<label for="checked-color">Checked color:</label>
				<input type="color" name="checked-color" bind:value={checkedColor} />
			</div>
			<div class="setting">
				<label for="transition-duration">Transition duration (s):</label>
				<div class="setting-input-wrapper">
					<input type="text" name="transition-duration" bind:value={transitionDuration} />
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('transitionDuration', 1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						+</button
					>
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('transitionDuration', -1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						-</button
					>
				</div>
			</div>
			<div class="setting">
				<label for="font-size">Font size (px):</label>
				<div class="setting-input-wrapper">
					<input type="text" name="font-size" bind:value={fontSize} />
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('fontSize', 1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						+</button
					>
					<button
						class="increment-btn"
						on:mousedown={() => startIncrement('fontSize', -1)}
						on:mouseup={stopIncrement}
						on:mouseleave={stopIncrement}
					>
						-</button
					>
				</div>
			</div>
		</div>

		<div class="component-section">
			<header class="component-header">
				<h1>Button</h1>
			</header>
			<div class="code-tabs">
				<button
					on:click={() => setActiveCodeTab('button', 'html')}
					class:active-code-tab={activeCodeTabs.button == 'html'}>HTML</button
				>
				<button
					on:click={() => setActiveCodeTab('button', 'css')}
					class:active-code-tab={activeCodeTabs.button == 'css'}>CSS</button
				>
			</div>
			<div class="component-card">
				<button class="component">Log in</button>
			</div>
			{#if activeCodeTabs.button == 'html'}
				<CodeBlock
					highlighter={highlighter}
					language="html"
					code={`<button class="neubrutalist">
  Log in
</button>`}
				/>
			{:else if activeCodeTabs.button == 'css'}
				<CodeBlock
					highlighter={highlighter}
					language="css"
					code={`.neubrutalist {
  padding: 1rem 2rem;
  font-family: "Inter", sans-serif;
  font-size: 1rem;
  border-radius: ${cornerRadius}px;
  border: ${borderWidth}px solid ${shadowColor};
  box-shadow: ${boxShadow};
  background-color: ${elementColor};
  color: ${shadowColor};
  transition-property: transform, box-shadow;
  transition-duration: ${transitionDuration}s;
}

button.neubrutalist:active {
  transform: translate(${shadowX}px, ${shadowY}px);
  box-shadow: none;
  outline: none;
}
`}
				/>
			{/if}
		</div>

		<div class="component-section">
			<header class="component-header">
				<h1>Input</h1>
			</header>
			<div class="code-tabs">
				<button
					on:click={() => setActiveCodeTab('input', 'html')}
					class:active-code-tab={activeCodeTabs.input == 'html'}>HTML</button
				>
				<button
					on:click={() => setActiveCodeTab('input', 'css')}
					class:active-code-tab={activeCodeTabs.input == 'css'}>CSS</button
				>
			</div>
			<div class="component-card">
				<div class="input-wrapper">
					<input type="text" name="Input" class="component" placeholder=" " />
					<label for="Input" class="input-label">Username</label>
				</div>
			</div>
			{#if activeCodeTabs.input == 'html'}
				<CodeBlock
					highlighter={highlighter}
					language="html"
					code={`<div class="input-wrapper">
  <input type="text" name="Input" class="neubrutalist" placeholder=" ">
  <label for="Input" class="input-label">Username</label>
</div>`}
				/>
			{:else if activeCodeTabs.input == 'css'}
				<CodeBlock
					highlighter={highlighter}
					language="css"
					code={`.neubrutalist {
  padding: 1rem 2rem;
  font-family: "Inter", sans-serif;
  font-size: 1rem;
  border-radius: ${cornerRadius}px;
  border: ${borderWidth}px solid ${shadowColor};
  box-shadow: ${boxShadow};
  background-color: ${elementColor};
  color: ${shadowColor};
  transition-property: transform, box-shadow;
  transition-duration: ${transitionDuration}s;
}

input.neubrutalist:active, input.neubrutalist:focus {
  transform: translate(${shadowX}px, ${shadowY}px);
  box-shadow: none;
  outline: none;
}

input.neubrutalist {
  padding: 1rem;
  position: relative;
  grid-row: 1;
  grid-column: 1;
}

.input-label {
  font-size: 1rem;
  font-family: "Inter", sans-serif;
  grid-row: 1;
  grid-column: 1;
  z-index: 1;
  pointer-events: none;
  transform: translateY(calc(${borderWidth}px + 1rem)) translateX(calc(${borderWidth}px + 0.5rem)) scale(1);
  background-color: ${elementColor};
  width: min-content;
  height: min-content;
  padding: 0 0.5rem;
  transition: transform ${transitionDuration}s, padding ${transitionDuration}s;
  color: ${shadowColor};
}

.input-wrapper {
  display: grid;
  grid-template-columns: 100%;
  grid-template-rows: 100%;
}

input.neubrutalist:not(:placeholder-shown) ~ .input-label {
  transform: translateY(-0.5rem) translateX(${borderWidth}px) scale(0.8);
  padding: calc(${borderWidth}px / 2) 0.5rem;
}

input.neubrutalist:focus ~ .input-label {
  transform: translateY(calc(-0.5rem + ${shadowY}px)) translateX(calc(${shadowX}px + ${borderWidth}px)) scale(0.8);
  padding: calc(${borderWidth}px / 2) 0.5rem;
}
`}
				/>
			{/if}
		</div>

		<div class="component-section">
			<header class="component-header">
				<h1>Checkbox</h1>
			</header>
			<div class="code-tabs">
				<button
					on:click={() => setActiveCodeTab('checkbox', 'html')}
					class:active-code-tab={activeCodeTabs.checkbox == 'html'}>HTML</button
				>
				<button
					on:click={() => setActiveCodeTab('checkbox', 'css')}
					class:active-code-tab={activeCodeTabs.checkbox == 'css'}>CSS</button
				>
			</div>
			<div class="component-card">
				<div class="switch-wrapper">
					<input type="checkbox" name="checkbox" class="switch" />
					<div class="switch-shadow"></div>
					<div class="switch-top"></div>
				</div>
			</div>
			{#if activeCodeTabs.checkbox == 'html'}
				<CodeBlock
					highlighter={highlighter}
					language="html"
					code={`<div class="switch-wrapper">
  <input type="checkbox" name="checkbox" class="switch">
  <div class="switch-shadow"></div>
  <div class="switch-top"></div>
</div>`}
				/>
			{:else if activeCodeTabs.checkbox == 'css'}
				<CodeBlock
					highlighter={highlighter}
					language="css"
					code={`.switch-wrapper {
  height: 2rem;
  width: 4rem;
  position: relative;
  border: ${borderWidth}px solid ${shadowColor};
  border-radius: 100rem;
  background: ${elementColor};
}

.switch {
  height: 100%;
  width: 100%;
  opacity: 0;
  margin: 0;
}

.switch-shadow {
  height: calc(100% + 0.4rem);
  width: 100%;
  position: absolute;
  bottom: 0;
  left: 0;
  border-radius: calc(1rem - ${borderWidth}px);
  overflow: hidden;
  pointer-events: none;
}

.switch-shadow::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  height: calc(95% - 0.4rem);
  width: 50%;
  border-radius: 100%;
  display: block;
  background: transparent;
  box-shadow: ${switchShadow};
  pointer-events: none;
  transition: left ${transitionDuration}s ease-in;
}

.switch-shadow::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  height: calc(100% - 0.4rem);
  width: 50%;
  border-radius: calc(1rem - ${borderWidth}px);
  display: block;
  background: ${checkedColor};
  transition: width ${transitionDuration}s ease-in;
}

.switch-top {
  height: 95%;
  width: 50%;
  box-sizing: border-box;
  position: absolute;
  top: calc(-0.4rem);
  left: 0;
  background: ${elementColor};
  border: ${borderWidth}px solid ${shadowColor};
  border-radius: 100%;
  pointer-events: none;
  transition: left ${transitionDuration}s ease-in;
}

.switch:checked ~ .switch-top, .switch:checked ~ .switch-shadow::after {
  left: 50%;
}

.switch:checked ~ .switch-shadow::before {
  width: 80%;
}
`}
				/>
			{/if}
		</div>

		<div class="component-section">
			<header class="component-header">
				<h1>Card</h1>
			</header>
			<div class="code-tabs">
				<button
					on:click={() => setActiveCodeTab('card', 'html')}
					class:active-code-tab={activeCodeTabs.card == 'html'}>HTML</button
				>
				<button
					on:click={() => setActiveCodeTab('card', 'css')}
					class:active-code-tab={activeCodeTabs.card == 'css'}>CSS</button
				>
			</div>
			<div class="component-card">
				<div class="component" style="width: 50%;">
					<h2>Card title</h2>
					<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet sint quam adipisci.</p>
				</div>
			</div>
			{#if activeCodeTabs.card == 'html'}
				<CodeBlock
					highlighter={highlighter}
					language="html"
					code={`<div class="neubrutalist">
</div>`}
				/>
			{:else if activeCodeTabs.card == 'css'}
				<CodeBlock
					highlighter={highlighter}
					language="css"
					code={`.neubrutalist {
  padding: 1rem 2rem;
  font-family: "Inter", sans-serif;
  font-size: 1rem;
  border-radius: ${cornerRadius}px;
  border: ${borderWidth}px solid ${shadowColor};
  box-shadow: ${boxShadow};
  background-color: ${elementColor};
  color: ${shadowColor};
}
`}
				/>
			{/if}
		</div>
	</div>

	<footer>
		<p>
			This application uses third-party materials under the following licenses:<br>
			Inter Font © <a href="https://github.com/rsms/inter">The Inter Project Authors (Rasmus Andersson)</a> <a href="https://openfontlicense.org/open-font-license-official-text/">SIL Open Font License 1.1</a><br>
			Instrument Serif Font © <a href="https://github.com/Instrument/instrument-serif">The Instrument Serif Project Authors (Rasmus Andersson)</a> <a href="https://openfontlicense.org/open-font-license-official-text/">SIL Open Font License 1.1</a><br>
			Saint Font © <a href="https://www.behance.net/dushnota">Liza Dushnota</a><br>
			<a href="https://github.com/shikijs/shiki">Shiki</a> © 2021 Pine Wu; © 2023 <a href="https://github.com/antfu">Anthony Fu</a> MIT License
		</p>
	</footer>
</main>

<style>
	#hero {
		height: 60vh;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		gap: 0.5rem;
		background:
			linear-gradient(transparent 6px, #2e2e2e 6px, transparent 7px) 0 0 / 20px 20px,
			linear-gradient(90deg, transparent 10px, #2e2e2e 10px, transparent 11px) 0 0 / 20px 20px;
	}

	#heading-g {
		letter-spacing: -0.15rem;
	}

	#hero p {
		font-family: 'Inter', sans-serif;
		max-width: 40rem;
		text-align: center;
	}

	#content {
		display: grid;
		grid-template-columns: 1fr 2fr;
		grid-template-rows: repeat(4, 400px);
		gap: 2rem;
		padding: 2rem;
	}

	#settings {
		grid-column: 1;
		grid-row: 1 / -1;
		display: flex;
		flex-direction: column;

		position: sticky;
		top: 1rem;
		left: 0;
		align-self: flex-start;

		gap: 1rem;
	}

	.setting {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.setting label {
		font-family: 'Inter', sans-serif;
	}

	.setting-input-wrapper {
		display: flex;
		gap: 0.2rem;
		align-items: center;
	}

	.setting input {
		padding: 0.3rem;
		border-radius: 0.5rem;
		border: 1px solid #484848;
		background-color: var(--black);
		color: var(--white);
		font-family: 'Inter', sans-serif;
		width: 5rem;
	}

	.setting input[type='color'] {
		padding: 0;
	}

	.setting input[type='color']::-moz-color-swatch {
		border: none;
	}

	.setting input[type='color']::-webkit-color-swatch {
		border: none;
	}

	.setting input[type='color']::-webkit-color-swatch-wrapper {
		padding: 0;
	}

	.increment-btn {
		height: 1.725rem;
		aspect-ratio: 1;
		border-radius: 0.5rem;
		border: 1px solid #484848;
		background-color: var(--black);
		color: var(--white);
		cursor: pointer;
		display: flex;
		justify-content: center;
		align-items: center;
		transition: background-color 0.1s;
	}

	.increment-btn:hover {
		background-color: #484848;
	}

	.increment-btn:active {
		background-color: #777777;
		border: 1px solid #777777;
	}

	.component-section {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: auto 1fr;
		gap: 0.5rem;
	}

	.code-tabs {
		display: flex;
		justify-content: flex-end;
		align-items: flex-end;
		gap: 0.5rem;
	}

	.code-tabs button {
		background-color: var(--black);
		color: var(--white);
		border: none;
		border-radius: 0.5rem;
		padding: 0.5rem;
		font-family: 'Inter', sans-serif;
		cursor: pointer;
	}

	.code-tabs button.active-code-tab {
		background-color: var(--white);
		color: var(--black);
	}

	.component-card {
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: var(--background-color);
		border-radius: 1rem;
		overflow: hidden;
	}

	footer {
		padding: 1rem;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		gap: 0.5rem;
		text-align: center;
		background:
			linear-gradient(transparent 6px, #2e2e2e 6px, transparent 7px) 0 0 / 20px 20px,
			linear-gradient(90deg, transparent 10px, #2e2e2e 10px, transparent 11px) 0 0 / 20px 20px;
	}

	footer a {
		color: #ffffff;
	}

	/* Components */
	.component {
		padding: var(--font-size-rem) calc(var(--font-size-rem) * 2);
		font-family: 'Inter', sans-serif;
		font-size: var(--font-size-rem);
		border-radius: var(--corner-radius);
		border: var(--border-width) solid var(--shadow-color);
		box-shadow: var(--box-shadow);
		background-color: var(--element-color);
		color: var(--shadow-color);
		transition-property: transform, box-shadow;
		transition-duration: var(--transition-duration);
	}

	input.component:active,
	input.component:focus,
	button.component:active {
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
		font-family: 'Inter', sans-serif;
		grid-row: 1;
		grid-column: 1;
		z-index: 1;
		pointer-events: none;
		transform: translateY(calc(var(--border-width) + var(--font-size-rem)))
			translateX(calc(var(--border-width) + var(--font-size-rem) / 2)) scale(1);
		background-color: var(--element-color);
		width: min-content;
		height: min-content;
		padding: 0 calc(var(--font-size-rem) / 2);
		transition:
			transform var(--transition-duration),
			padding var(--transition-duration);
		color: var(--shadow-color);
	}

	.input-wrapper {
		display: grid;
		grid-template-columns: 100%;
		grid-template-rows: 100%;
	}

	input.component:not(:placeholder-shown) ~ .input-label {
		transform: translateY(calc(var(--font-size-rem) * -0.5)) translateX(var(--border-width))
			scale(0.8);
		padding: calc(var(--border-width) / 2) calc(var(--font-size-rem) / 2);
	}

	input.component:focus ~ .input-label {
		transform: translateY(calc(var(--font-size-rem) * -0.5 + var(--shadow-y)))
			translateX(calc(var(--shadow-x) + var(--border-width))) scale(0.8);
		padding: calc(var(--border-width) / 2) calc(var(--font-size-rem) / 2);
	}

	/* Checkbox */
	.switch-wrapper {
		height: calc(var(--font-size-rem) * 2);
		width: calc(var(--font-size-rem) * 4);
		position: relative;
		border: var(--border-width) solid var(--shadow-color);
		border-radius: calc(var(--font-size-rem) * 10);
		background: var(--element-color);
	}

	.switch {
		height: 100%;
		width: 100%;
		opacity: 0;
		margin: 0;
	}

	.switch-shadow {
		height: calc(100% + var(--font-size-rem) * 0.4);
		width: 100%;
		position: absolute;
		bottom: 0;
		left: 0;
		border-radius: calc(var(--font-size-rem) - var(--border-width));
		overflow: hidden;
		pointer-events: none;
	}

	.switch-shadow::after {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		height: calc(95% - var(--font-size-rem) * 0.4);
		width: 50%;
		border-radius: 100%;
		display: block;
		background: transparent;
		box-shadow: var(--switch-shadow);
		pointer-events: none;
		transition: left var(--transition-duration) ease-in;
	}

	.switch-shadow::before {
		content: '';
		position: absolute;
		bottom: 0;
		left: 0;
		height: calc(100% - var(--font-size-rem) * 0.4);
		width: 50%;
		border-radius: calc(var(--font-size-rem) - var(--border-width));
		display: block;
		background: var(--checked-color);
		transition: width var(--transition-duration) ease-in;
	}

	.switch-top {
		height: 95%;
		width: 50%;
		box-sizing: border-box;
		position: absolute;
		top: calc(var(--font-size-rem) * -0.4);
		left: 0;
		background: var(--element-color);
		border: var(--border-width) solid var(--shadow-color);
		border-radius: 100%;
		pointer-events: none;
		transition: left var(--transition-duration) ease-in;
	}

	.switch:checked ~ .switch-top,
	.switch:checked ~ .switch-shadow::after {
		left: 50%;
	}

	.switch:checked ~ .switch-shadow::before {
		width: 80%;
	}
</style>
