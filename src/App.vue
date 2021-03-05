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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
