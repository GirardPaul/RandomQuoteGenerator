<script setup>
import Citation from "../components/Citation.vue";
import Author from "../components/Author.vue";
import { onMounted, ref } from "vue";
const url = "https://quote-garden.onrender.com/api/v3/quotes";
const quotes = ref([]);
const authorName = ref("");
const genre = ref("");
const isLoading = ref(false);
const isSingleQuote = ref(true);

async function getTotalNbPages() {
  const response = await fetch(url);
  if (response.ok) {
    const { pagination } = await response.json();
    console.log(pagination);
    return pagination.totalPages;
  } else {
    console.log("Failed to get total number of pages");
  }
}

async function generateQuote(author = null) {
  isLoading.value = true;
  const params = new URLSearchParams();
  if (author) {
    params.append("author", author);
  } else {
    const totalNbPages = await getTotalNbPages();
    const randomPage = Math.floor(Math.random() * totalNbPages) + 1;
    params.append("page", randomPage);
    params.append("limit", 1);
  }

  const response = await fetch(`${url}?${params}`);

  if (response.ok) {
    const { data } = await response.json();
    isSingleQuote.value = data.length === 1;
    quotes.value = data;
    authorName.value = data[0].quoteAuthor;
    genre.value = data[0].quoteGenre;
    isLoading.value = false;
  } else {
    console.log("Failed to get quotes");
  }
}

onMounted(() => {
  generateQuote();
});
</script>
<template>
  <header>
    <div @click="generateQuote()" class="flex align-center pointer">
      <p>Random</p>
      <span :class="['material-icons', isLoading ? 'rotate' : '']"
        >autorenew</span
      >
    </div>
  </header>
  <section
    v-if="quotes.length"
    :class="[
      'container',
      'flex',
      'column',
      isSingleQuote ? 'margin-single-quote' : 'margin-multiple-quote',
    ]"
  >
    <h3
      v-if="!isSingleQuote"
      :class="[!isSingleQuote ? 'margin-bottom-quote' : '']"
    >
      {{ authorName }}
    </h3>
    <div class="quotes-container">
      <Citation v-for="quote in quotes" :key="quote._id" :quote="quote" />
    </div>
    <Author
      v-if="isSingleQuote"
      @randomAdditionnalQuotes="generateQuote"
      class="mt-author"
      :name="authorName"
      :genre="genre"
    />
  </section>
</template>

<style scoped>
.quotes-container {
  display: grid;
  /* each row contain one quote  */
  gap: 14rem;
}
.margin-bottom-quote {
  margin-bottom: 14rem;
}
.mt-author {
  margin-top: 16rem;
}
h3 {
  font-size: 2.5rem;
  font-weight: 700;
  color: #333;
}
header {
  display: flex;
  justify-content: flex-end;
  margin: 3.1rem 10rem 0 10rem;
}

header p {
  font-size: 1.8rem;
  font-weight: 500;
  color: #333;
}
header span {
  color: #333;
  font-size: 1.8rem;
  margin-left: 1.1rem;
}

header div:hover span {
  animation: rotate 1.5s linear infinite;
}

.rotate {
  animation: rotate 1.5s linear infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.container {
  display: flex;
  align-items: flex-start;
  margin: 22rem 40rem 22rem 40rem;
}

.margin-single-quote {
  margin: 22rem 40rem 22rem 40rem;
}

.margin-multiple-quote {
  margin: 4.8rem 40rem 14rem 40rem;
}

@media screen and (min-width: 901px) and (max-width: 1200px) {
  .margin-single-quote {
    margin: 10rem 20rem 10rem 20rem;
  }
  .margin-multiple-quote {
    margin: 4.8rem 20rem 14rem 20rem;
  }

  header {
    margin: 3.1rem 7.5rem 0 7.5rem;
  }
}

@media screen and (min-width: 468px) and (max-width: 900px) {
  .margin-single-quote {
    margin: 7.5rem 10rem 7.5rem 10rem;
  }
  .margin-multiple-quote {
    margin: 4.8rem 10rem 14rem 10rem;
  }

  .mt-author {
    margin-top: 8rem;
  }

  header {
    margin: 3.1rem 5rem 0 5rem;
  }
}

@media screen and (max-width: 467px) {
  .margin-single-quote {
    margin: 5rem;
  }
  .margin-multiple-quote {
    margin: 4.8rem 5rem 14rem 5rem;
  }

  header {
    margin: 3.1rem 2.5rem 0 2.5rem;
  }
}
</style>
