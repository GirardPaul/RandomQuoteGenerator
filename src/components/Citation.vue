<script setup>
defineProps({
  quote: Object,
});
</script>
<template>
  <div>
    <p>“{{ quote.quoteText }}“</p>
  </div>
</template>

<style scoped>
div {
  padding-left: 10rem;
  border-left: 0.8rem solid #f7df94;
}
p {
  font-size: 2.5rem;
  color: #000;
  font-weight: 500;
}
</style>
