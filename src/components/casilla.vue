<template>
  <button>
    {{ figura }}
  </button>
</template>

<script setup lang="ts">
defineProps({
  figura: String,
});
</script>

<style scoped></style>
