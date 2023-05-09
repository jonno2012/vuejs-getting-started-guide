<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import TodoList from "./components/TodoList.vue";

</script>
<script>
export default {
  data() {
    return {
      showLogo: true
    }
  }
}
</script>
<template>
  <header>
    <img v-if="showLogo" alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld
          msg="You did it!"
          tester="http://jrhilldev.com"
          button-disabled=""
          message="this will be backwards"
          show-abusive-message="false"
      />
      <br>
      <TodoList @showa="(visible) => showLogo = visible" class="willBeAppliedToRootElementOfTemplate">Some text</TodoList>

    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
