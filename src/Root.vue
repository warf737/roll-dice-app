<script>
import CharacterMain from './components/CharacterMain.vue';

export default {
  name: 'App',
  components: {
		CharacterMain,
  }
}
</script>

<template>
	<div id="app">
		<character-main/>
	</div>
</template>

<style lang="scss">
html, body {
	padding: 0;
	margin: 0;
}

html {
	box-sizing: border-box;

	/* Должно отключить увеличение шрифта на некоторых Iphone*/
	-ms-text-size-adjust: 100%;
	-webkit-text-size-adjust: 100%;
	-moz-text-size-adjust: 100%;
}

*, *:before, *:after {
	box-sizing: inherit;
}

body {
	width: auto;
	margin: 0 auto;
	font-size: 16px;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	background-color: var(--grey);
	font-weight: normal;
	color: var(--black);
	overflow-y: scroll;
	text-rendering: optimizeLegibility;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}

img {
	max-width: 100%;
	height: auto;
}

p, ul, li, h1, h2, h3, table, tr, td {
	margin: 0;
	padding: 0;
}

a {
	display: block;
	color: #42b983;
	text-decoration: none;
}

:root {
	--black: #2c3e50;
	--grey: #f3f6f8;
	--mint: #42b983,
}

ul {
	list-style: none;
}

fieldset, input {
	margin: 0;
	padding: 0;
	border: none;
	background: none;
}

input, label, pre {
	display: block;
	font-family: Avenir, Helvetica, Arial, sans-serif;
}

.nobr {
	white-space: nowrap;
}
.hidden {
	position: absolute;
	width: 1px;
	height: 1px;
	margin: -1px;
	padding: 0;
	border: 0;
	clip: rect(0 0 0 0);
	overflow: hidden;
}
</style>
