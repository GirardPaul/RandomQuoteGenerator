<script setup>
defineProps({
  name: String,
  genre: String,
});
const emit = defineEmits(["randomAdditionnalQuotes"]);
const random = (name) => {
  emit("randomAdditionnalQuotes", name);
};
</script>
<template>
  <div class="flex full justify-between align-center row">
    <div class="flex column">
      <h3>{{ name }}</h3>
      <p>{{ genre }}</p>
    </div>
    <span @click="random(name)" class="material-icons pointer"
      >trending_flat</span
    >
  </div>
</template>

<style scoped>
.row {
  padding: 5rem 3rem;
}

.material-icons {
  font-size: 3.6rem;
  color: #fff;
}
h3 {
  font-size: 2.4rem;
  font-weight: 700;
  color: #4f4f4f;
}
p {
  font-size: 1.4rem;
  font-weight: 500;
  color: #828282;
  margin-top: 0.8rem;
}

@media screen and (min-width: 769px) {
  .row:hover {
    background-color: #333333;
  }

  .row:hover h3 {
    color: #fff;
  }
}

@media screen and (max-width: 768px) {
  .row {
    background-color: #333333;
  }
  .row h3 {
    color: #fff;
  }
}
</style>
