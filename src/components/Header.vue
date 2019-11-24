<template>
  <b-nav tabs>
    <b-nav-item disable>Fansy Quiz App</b-nav-item>
    <b-nav-item disable>Counter {{ numCorrect }}/ {{ numTotal }}</b-nav-item>
  </b-nav>
</template>

<script>
export default {
  props: ["numCorrect", "numTotal"]
};
</script>