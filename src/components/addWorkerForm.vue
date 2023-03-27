<template>
  <v-form class="pa-10" width="500px" @submit.prevent="handleSubmit" ref="form">
    <v-row><v-col><h2>Добавить нового работника</h2></v-col></v-row>

    <v-row>
      <v-col>
        <v-text-field
          v-model="fullName"
          :rules="[ rules.required, rules.name]"
          variant="outlined"
          label="ФИО"
        />
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-select
          label="Пол"
          variant="outlined"
          :items="gender"
          :rules="[rules.required]"
          items-title="title"
          item-value="id"
          v-model="selectedGender"
      /></v-col>
      <v-col>
        <v-text-field
          variant="outlined"
          label="Дата рождения"
          type="date"
          :max="getCurrentDate()"
          v-model="dateBirth"
      /></v-col>

      <v-col>
        <v-text-field
          v-model="age"
          variant="outlined"
          label="Возраст"
          type="number"
          readonly
        />
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-select
          label="Гражданство"
          variant="outlined"
          :items="county"
          :rules="[rules.required]"
          items-title="title"
          item-value="id"
          v-model="selectedСounty"
      /></v-col>
      <v-col>
        <v-text-field
          variant="outlined"
          label="Город"
          v-model="address"
          :rules="[rules.required]"
        />
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-select
          label="Тип договора"
          variant="outlined"
          :items="type_contract"
          :rules="[rules.required]"
          items-title="title"
          item-value="id"
          v-model="selectedContractType"
      /></v-col>

      <v-col>
        <v-select
          label="Должность"
          variant="outlined"
          :items="position"
          :rules="[rules.required]"
          items-title="title"
          item-value="id"
          v-model="selectedPosition"
      /></v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-text-field
          variant="outlined"
          label="ИНН"
          maxlength="12"
          :rules="[ rules.required, rules.onlyNumber, rules.innLength]"
          v-model="inn"
        />
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-select
          label="Статус"
          variant="outlined"
          id="county"
          :items="tags"
          :rules="[rules.required]"
          items-title="title"
          item-value="id"
          v-model="selectedTag"
      /></v-col>

      <v-col>
        <v-text-field
          variant="outlined"
          label="Описание статуса"
          :rules="[rules.required]"
          v-model="statusDescription"
      /></v-col>
    </v-row>

    <v-row
      ><v-col
        ><v-btn
          class="add-button w-100 py-5 bg-light-blue-accent-3 h-auto"
          type="submit"
          >Добавить</v-btn
        ></v-col
      ></v-row
    >
  </v-form>
</template>

<script setup>
import { ref, computed } from "vue";
import { county, gender, position, type_contract, tags } from "@/constants";
import { getCurrentDate } from "@/utils/getCurrentDate";

const emit = defineEmits(["submitted"]);
const form = ref(null);

const fullName = ref(null);
const selectedGender = ref(null);
const dateBirth = ref(getCurrentDate());

const selectedСounty = ref(null);
const address = ref(null);

const selectedContractType = ref(null);
const selectedPosition = ref(null);

const inn = ref(null);

const selectedTag = ref(null);
const statusDescription = ref(null);

const age = computed(() => {
  return (
    ((new Date().getTime() - new Date(dateBirth.value)) /
      (24 * 3600 * 365.25 * 1000)) |
    0
  );
}, 0);

const rules = {
  required: (value) => !!value || "Обязательное поле",
  innLength: (value) =>
    (value && value.length === 12) || "ИНН содержит 12 символов",
  onlyNumber: (value) => /^[0-9]+$/.test(value) || "Введен недопустимый символ",
  name: (value) =>
    /^[а-я -]+\s[а-я -]+\s[а-я -]+$/i.test(value) ||
    "Введите полное корректное ФИО",
  onlyPositive: (value) => value > 0 || "Значение должно быть больше 0",
};

const handleSubmit = () => {
  form.value.validate().then((res) => {
    if (res.valid)
        emit("submitted", {
        full_name: fullName.value,
        inn: inn.value,
        address: address.value,
        date_birth: dateBirth.value,
        age: age.value,
        type_contract: type_contract.find((item)=> item.id===selectedContractType.value),
        type_contract_id: selectedContractType.value,
        gender: gender.find((item)=> item.id===selectedGender.value),
        gender_id: selectedGender.value,
        county: county.find((item)=> item.id===selectedСounty.value),
        county_id: selectedСounty.value,
        position: position.find((item)=> item.id===selectedPosition.value),
        position_id: selectedPosition.value,
        status: { tag_id: selectedTag.value, tag: tags.find((item)=> item.id===selectedTag.value), description: statusDescription.value },
      });}
  );
};
</script>

<style scoped>
.v-col {
  padding-top: 3px;
  padding-bottom: 3px;
}
</style>
