<template>
  <v-form class="ma-7" ref="form">
    <h2 class="mb-5 font">Фильтр</h2>
    <v-row>
      <v-col>
        <label for="county" class="d-block mb-2 font-weight-medium">Гражданство</label>
        <v-select
          placeholder="Все страны"
          variant="outlined"
          id="county"
          :items="county"
          items-title="title"
          item-value="id"
          v-model="filterCounty"
        />
      </v-col>
      <v-col>
        <label for="gender" class="d-block mb-2 font-weight-medium">Пол</label>
        <v-select
          placeholder="Без разницы"
          variant="outlined"
          id="gender"
          :items="gender"
          items-title="title"
          item-value="id"
          v-model="filterGender"
        />
      </v-col>
    </v-row>

    <label for="position" class="d-block mb-2 font-weight-medium">Должность</label>
    <v-select
      placeholder="Все должности"
      variant="outlined"
      id="position"
      :items="position"
      items-title="title"
      item-value="id"
      v-model="filterPosition"
    ></v-select>

    <p class="font-weight-medium">Тип договора</p>
    <v-checkbox
      v-for="contract in type_contract"
      v-model="filterContract"
      :label="contract.title"
      :value="contract.id"
      color="light-blue-accent-3"
      hide-details
      class="checkbox"
    />

    <v-divider />

    <v-row>
      <v-col>
        <v-btn
          class="d-flex py-4 mt-5 bg-success h-auto w-100"
          @click="handleSubmit"
        >
          Применить
        </v-btn>
      </v-col>

      <v-col>
        <v-btn
          class="d-flex py-4 mt-5 bg-grey-darken-1 h-auto w-100"
          @click="reset"
        >
          Очистить
        </v-btn>
      </v-col>
    </v-row>
  </v-form>
</template>

<script setup>
import { ref } from "vue";
import { county, gender, position, type_contract } from "@/constants";

const emit = defineEmits(["submitted"]);

const filterCounty = ref(null);
const filterGender = ref(null);
const filterPosition = ref(null);
const filterContract = ref([]);
const form = ref(null);

const handleSubmit = () => {
  emit("submitted", {
    countyOption: filterCounty.value,
    genderOption: filterGender.value,
    positionOption: filterPosition.value,
    contractOption: filterContract.value,
  });
};

const reset = () => {
  form.value.reset();
  filterContract.value = [];
  emit('submitted', null)
};
</script>

<style scoped></style>
