<template>
  <!-- бейджи фильтров -->
  <div class="d-flex align-center">
    <v-chip
      @click="resetSelectedBadge"
      variant="outlined"
      class="chip-reset mr-2"
      :class="{ 'chip-rest_active': selectedBadge.length === 0 }"
    >
      весь список
    </v-chip>
    <v-chip-group
      multiple
      variant="outlined"
      class="filter-chip"
      v-model="selectedBadge"
    >
      <v-chip v-for="tag in tags" :value="tag.id" :key="tag.id" :class="`${tag.slug}`">
        {{ tag.title }}
      </v-chip>
    </v-chip-group>
  </div>

  <ul class="d-flex flex-column list">
    <li v-for="worker in workers">
      <WorkerCard
        :worker="worker"
        :key="worker.inn"
      />
    </li>
  </ul>
</template>

<script setup>
import { ref, watch } from "vue";
import WorkerCard from "./workerCard.vue";

import {tags} from '@/constants';

const props = defineProps({
  workers: Array,
});
const emit = defineEmits(["selectBadge"]);

const selectedBadge = ref([]);

const resetSelectedBadge = () => {
  selectedBadge.value = [];
};

watch(selectedBadge, () => {
  emit("selectBadge", [...selectedBadge.value]);
});
</script>

<style scoped>

.list {
  list-style: none;
  gap: 15px;
}
.pr {
  color: #e2bd06;
}
.pr.v-chip--selected {
  color: white;
  background-color: #e2bd06;
}
.cr {
  color: #e52e2e;
}
.cr.v-chip--selected {
  color: white;
  background-color: #e52e2e;
}
.cm {
  color: #00b6ed;
}
.cm.v-chip--selected {
  color: white;
  background-color: #00b6ed;
}
.dn {
  color: #00ae5b;
}
.dn.v-chip--selected {
  color: white;
  background-color: #00ae5b;
}
.chip-reset {
  color: #b0bcc7;
}
.chip-rest_active {
  color: white;
  background-color: #b0bcc7;
}
</style>
