<template>
  <div class="small-screen-warning" v-if="isSmallScreen">
    Please use a device with a screen wider than 650 pixels for the best
    experience.
  </div>
  <div class="main-page" v-loading.fullscreen.lock="isLoading">
    <MyFiltration @filter="handleFilter" @reset="resetFilters" />
    <div class="cards__wrapper">
      <div class="cards__inner-wrapper">
        <template v-if="characters.length > 0">
          <div v-for="character in characters" :key="character.id">
            <CharacterCard :character="character" />
          </div>
        </template>
        <div v-else-if="!isLoading" class="no-data">Нет данных</div>
      </div>
    </div>
    <MyPagination
      v-if="characters.length > 0"
      :total-characters="totalCharacters"
      :current-page="currentPage"
      :page-size="pageSize"
      @page-change="handlePageChange"
    />
  </div>
</template>

<script setup>
import { ref, onMounted, watch, computed } from "vue";
import MyFiltration from "@/components/MyFiltration.vue";
import CharacterCard from "@/components/CharacterCard.vue";
import MyPagination from "@/components/MyPagination.vue";

const characters = ref([]);
const currentPage = ref(1);
const totalCharacters = ref(0);
const pageSize = 20;
const currentFilters = ref({});
const isLoading = ref(false);

const isSmallScreen = computed(() => window.innerWidth <= 650);

async function fetchCharacters(filters = currentFilters.value) {
  isLoading.value = true;
  const params = new URLSearchParams({ page: currentPage.value.toString() });
  if (filters.name) params.append("name", filters.name);
  if (filters.status) params.append("status", filters.status);

  const response = await fetch(
    `https://rickandmortyapi.com/api/character?${params}`
  );
  const data = await response.json();
  if (data && data.info && data.results.length > 0) {
    characters.value = data.results;
    totalCharacters.value = data.info.count;
  } else {
    characters.value = [];
    totalCharacters.value = 0;
  }
  isLoading.value = false;
}

function handleFilter(filters) {
  currentFilters.value = filters;
  currentPage.value = 1;
  fetchCharacters(filters);
}

function resetFilters() {
  currentFilters.value = {};
  fetchCharacters();
}

function handlePageChange(newPage) {
  currentPage.value = newPage;
  fetchCharacters();
}

watch(currentFilters, fetchCharacters);
onMounted(fetchCharacters);
</script>

<style scoped>
.main-page {
  min-height: 100vh;
  width: 100%;
}
.cards__wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 4.5rem 0;
  background: rgb(39, 43, 51);
  min-height: calc(50vh - 60px);
}

.cards__inner-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  max-width: 1920px;
}

.no-data {
  color: white;
  font-size: 25px;
  font-weight: 700;
  text-align: center;
  width: 100%;
  padding: 20px;
}

.small-screen-warning {
  display: none;
}

@media (max-width: 650px) {
  .small-screen-warning {
    font-size: 20px;
    display: flex;
    color: var(--main-text);
    justify-content: center;
    align-items: center;
    position: fixed;
    padding: 10px;
    height: 100%;
    text-align: center;
  }
  .main-page {
    display: none;
  }
}
</style>
