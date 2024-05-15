<template>
  <div class="character-card">
    <div class="character-card__img-wrapper">
      <img :src="character.image" :alt="character.name" />
    </div>
    <div class="character-card__content-wrapper">
      <div class="section section--first">
        <h2>{{ character.name }}</h2>
        <div class="status">
          <span
            class="status__icon"
            :class="{
              'status__icon--alive': character.status === 'Alive',
              'status__icon--dead': character.status === 'Dead',
              'status__icon--unknown': character.status === 'unknown',
            }"
          ></span>
          {{ character.status }} - {{ character.species }}
        </div>
      </div>
      <div class="section">
        <span class="additional-text">Last known location:</span>
        <span class="location">{{ character.location.name }}</span>
      </div>
      <div class="section section--last">
        <span class="additional-text">First seen in:</span>
        <span class="episode">{{ episodeTitle }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, ref, onMounted } from "vue";

const props = defineProps({
  character: Object,
});

const episodeTitle = ref("");

async function getEpisodeTitle(url) {
  const episodeId = url.split("/").pop();
  try {
    const response = await fetch(
      `https://rickandmortyapi.com/api/episode/${episodeId}`
    );
    const data = await response.json();
    episodeTitle.value = data.name;
  } catch (error) {
    console.error("Error fetching episode details:", error);
  }
}

onMounted(() => {
  if (props.character.episode && props.character.episode.length > 0) {
    getEpisodeTitle(props.character.episode[0]);
  }
});
</script>

<style scoped>
.character-card {
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: var(--bg-color-card);
  border-radius: 0.5rem;
  margin: 0.75rem;
  box-shadow: var(--box-shadow-01) 0px 4px 6px -1px,
    var(--box-shadow-02) 0px 2px 4px -1px;
}

.character-card__img-wrapper {
  flex: 2 1 0%;
  width: 100%;
}

.character-card__img-wrapper img {
  width: 100%;
  height: 100%;
  margin: 0px;
  opacity: 1;
  transition: opacity 0.5s ease 0s;
  object-fit: cover;
  object-position: center center;
}

.character-card__content-wrapper {
  flex: 3 1 0%;
  position: relative;
  padding: 0.75rem;
  color: var(--main-text);
  display: flex;
  flex-direction: column;
}

.section {
  flex: 1 1 0%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.section--first {
  justify-content: flex-start;
}

.section--last {
  justify-content: flex-end;
}

h2 {
  font-size: 27px;
  font-weight: 800;
  margin: 0;
  padding: 0;
}

.status {
  display: flex;
  align-items: center;
}
.status__icon {
  height: 9px;
  width: 9px;
  margin-right: 0.375rem;
  border-radius: 50%;
}

.status__icon--alive {
  background: var(--bg-color-element-01);
}

.status__icon--dead {
  background: var(--bg-color-element-02);
}

.status__icon--unknown {
  display: none;
}

.additional-text {
  color: var(--additional-text);
}

span {
  font-size: 16px;
  font-weight: 500;
}

.location,
.episode {
  font-size: 18px;
  font-weight: 400;
  line-height: 1.625;
}
</style>
