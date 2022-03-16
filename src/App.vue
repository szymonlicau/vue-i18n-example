<template>
  <div id="app">
    <select
      :value="$i18n.locale"
      @change="changeLocale($event.target.value)"
    >
      <option
        v-for="locale in locales"
        :key="locale.id"
        :value="locale.id"
      >
        {{ locale.name }}
      </option>
    </select>

    <Cart />
  </div>
</template>

<script>
import Cart from './components/Cart.vue'

export default {
  name: 'App',
  components: {
    Cart
  },

  computed: {
    locales () {
      return ['en', 'pl'].map(locale => {
        return {
          id: locale,
          name: locale.toUpperCase()
        }
      });
    }
  },

  methods: {
    changeLocale (locale) {
      // It's important to change it at the $root
      // otherwise the change will be local
      this.$root.$i18n.locale = locale;
    }
  }
}
</script>

<style lang="scss">
:root {
  --text-color: #2c3e50;
  --border-color: black;
  --primary-color: #41b883;
  --primary-hover-color: #2c7d59;
  --white: white;
}

html {
  font-size: 62.5%;
}

body {
  font-size: 1.6rem;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--text-color);
  max-width: 128rem;
  margin: 6rem auto;
}
</style>
