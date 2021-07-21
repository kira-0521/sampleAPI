<template>
  <div>
    <h1>{{ hello }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      hello: "hello world",
    };
  },
};
</script>

<style>
h1 {
  font-size: 32px;
}
</style>
