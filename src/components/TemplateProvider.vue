<template>
  <div>
    <header>
      <h1>Multiple Choice Quiz</h1>
      <small>VueJS 3, Composition API, No Vuex</small>
    </header>
    <main>
      <div class="slot">
        <slot />
      </div>
    </main>
    <footer>
      Developed by
      <a href="https://github.com/gayret">Safa Gayret</a>
    </footer>
  </div>
</template>

<style scoped>
header,
footer {
  text-align: center;
  margin-bottom: 2em;
}

main {
  display: flex;
}

.slot {
  margin: auto;
}

footer {
  padding: 4em;
  font-size: 0.8em;
}
</style>
