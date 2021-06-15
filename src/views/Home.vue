<template>
  <div class="home">
    <div v-if="tavern.tavern">
      <span>Tavern: </span>
      <a :href="tavernLink" target="_blank">{{ tavern.tavern.title }}</a>
    </div>
    <div v-if="tavern.excuse">
      <span>Excuse: </span> <span>{{ tavern.excuse.text }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref, computed } from "vue";
import { ITavernResponse } from "@/types/gateway/types"; // @ is an alias to /src

export default defineComponent({
  name: "Home",

  setup() {
    const tavern = ref<ITavernResponse>({
      tavern: null,
      excuse: null,
    });
    onMounted(async () => {
      const response = await fetch("http://localhost:3000/tavern");
      const gettedTavern: ITavernResponse = await response.json();
      tavern.value = gettedTavern;
    });

    const tavernLink = computed(
      () =>
        `https://www.google.com/maps/place/${tavern.value.tavern?.title}/@${tavern.value.tavern?.longitude},${tavern.value.tavern?.latitude}z`
    );

    return {
      tavern,
      tavernLink,
    };
  },
});
</script>
