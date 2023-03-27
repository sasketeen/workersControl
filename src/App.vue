<template>
  <v-app style="background: none">
    <v-main>
      <v-sheet
        class="d-flex"
        max-width="1820px"
        width="100%"
        style="background: none; gap: 40px; align-self: center"
      >
        <v-card width="66%" class="rounded-0 py-7">
          <div class="px-7 pb-7">
            <v-text-field
              v-model="searchData"
              variant="outlined"
              append-inner-icon="mdi-magnify"
              label="Поиск сотрудника"
              hint="Например Иванов Иван"
              persistent-hint
              class="search-input"
              density="comfortable"
            />
          </div>

          <v-divider />

          <div class="py-7 px-10">
            <h2>Список сотрудников</h2>

            <WorkerList
              :workers="filteredWorkers.slice(0, maxWorkers)"
              @selectBadge="handleSelectBadge"
            ></WorkerList>

            <div
              v-if="maxWorkers < workers.length && maxWorkers < filteredWorkers.length"
              class="d-flex justify-center mt-5"
            >
              <v-btn
                prepend-icon="mdi-cached"
                variant="outlined"
                color="primary"
                @click="loadMoreWorkers"
                >Показать еще</v-btn
              >
            </div>
          </div>
        </v-card>

        <v-card width="32%" class="control-card" height="min-content">
          <div class="ma-7">
            <v-btn
              prepend-icon="mdi-account-plus"
              class="add-button w-100 py-5 bg-light-blue-accent-3 h-auto"
              @click="overlayOpen=!overlayOpen"
              >Добавить нового сотрудника</v-btn
            >
          </div>

          <v-divider />

          <filter-form @submitted="handleSelectFilterOption" />
        </v-card>

      </v-sheet>
    </v-main>

    <v-overlay
      v-model="overlayOpen"
      class="d-flex align-center justify-center"
    >
      <v-card width="700px">
        <add-worker-form @submitted="handleAddWorkers"/>
      </v-card>
    </v-overlay>

  </v-app>
</template>

<script setup>
import { ref, computed } from "vue";
import WorkerList from "./components/workerList.vue";
import FilterForm from "./components/filterForm.vue";
import AddWorkerForm from "./components/addWorkerForm.vue";

import { workers } from "@/constants";

const searchData = ref('');
const selectedBadge = ref([]);
const filterOption = ref(null);
const maxWorkers = ref(4);
const overlayOpen = ref(false);

const handleSelectBadge = (data) => {
  selectedBadge.value = data;
};

const handleSelectFilterOption = (data) => {
  filterOption.value = data;
};

const loadMoreWorkers = () => {
  maxWorkers.value *= 2;
};

const handleAddWorkers = (data) => {
  overlayOpen.value = false;
  workers.value = [data, ...workers.value]
}

// фильтрация пользователей
const filteredWorkers = computed(() => {
  let result = [...workers.value];
  if (filterOption.value) {
    if (filterOption.value.countyOption)
      result = result.filter(
        (worker) => filterOption.value.countyOption === worker.county_id
      );
    if (filterOption.value.genderOption)
      result = result.filter(
        (worker) => filterOption.value.genderOption === worker.gender_id
      );
    if (filterOption.value.positionOption)
      result = result.filter(
        (worker) => filterOption.value.positionOption === worker.position_id
      );
    if (filterOption.value.contractOption.length)
      result = result.filter((worker) =>
        filterOption.value.contractOption.includes(worker.type_contract_id)
      );
  }
  if (selectedBadge.value.length)
    result = result.filter((worker) =>
      selectedBadge.value.includes(worker.status.tag_id)
    );
    if (searchData.value) {
      result = result.filter((worker) => worker.full_name.toLocaleLowerCase().startsWith(searchData.value.toLocaleLowerCase()))
    }
  return result;
});
</script>

<style>
.control-card {
  position: sticky;
  top: 41px;
}
.submit-wrapper {
  gap: 20px;
}
</style>
