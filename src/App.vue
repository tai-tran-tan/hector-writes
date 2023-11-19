<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import { ref } from "vue";
import Home from "./components/Home.vue";
import Cell from "./components/Cell.vue";

const DEFAULT_SELECTED = 10;

const input = ref("");
const selected = ref(DEFAULT_SELECTED);
const sizes = { row: 4, col: 7 }; // rows and columns of alphabet table

const speech = ref("");

function onKeyEvent(e: KeyboardEvent) {
  const code = e.code;
  console.log("pressed", code);
  input.value = code;
  const update = updateValue(code);
  selected.value = Math.max(
    0,
    Math.min(sizes.row * sizes.col - 1, selected.value + update)
  );
}
function updateValue(code: String) {
  switch (code) {
    case "ArrowUp":
      return -sizes.col;
    case "ArrowDown":
      return sizes.col;
    case "ArrowLeft":
      return -1;
    case "ArrowRight":
      return 1;
    default:
      return 0;
  }
}

function commit() {
  speech.value += toChar(selected.value);
  input.value = ''
}

const toChar = (code: number) => String.fromCharCode(97 + code);
const cell = (code: number) =>
  ref({ label: toChar(code), selected: code == selected.value });
</script>

<template >
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="@/assets/logo.svg"
      width="125"
      height="125"
    />

    <!-- <Home msg="Hector says" /> -->
    <div class="wrapper">
      <p :key="r" v-for="r in [...Array(sizes.row).keys()]">
        <Cell
          :key="c"
          v-for="c in [...Array(sizes.col).keys()]"
          v-bind="cell(sizes.col * r + c).value"
        >
        </Cell>
      </p>

      <input
        class="input-box"
        @keyup.space="commit"
        @keyup.up="onKeyEvent"
        @keyup.down="onKeyEvent"
        @keyup.left="onKeyEvent"
        @keyup.right="onKeyEvent"
        v-model="input"
      />
      <input class="speech-box" type="text" disabled v-model="speech"/>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.input-box {
  width: 5em;
}
.speech-box {
  width: 20em;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
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

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
