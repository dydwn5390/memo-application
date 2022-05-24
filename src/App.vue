<template>
  <div id="app">
    <app-header/>
    <memo-app/>
  </div>
</template>

<style>
@import "./styles/reset.css";

#app {
  width: 560px;
  margin: 0 auto;
}
</style>

<script>
import AppHeader from "@/components/AppHeader";
import MemoApp from "@/components/MemoApp";

export default {
  name: 'app',
  components: {
    MemoApp,
    AppHeader
  }
}
</script>