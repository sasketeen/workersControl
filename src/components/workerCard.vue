<template>
  <v-card
    class="worker-card w-100 pa-6 d-flex align-start flex-column"
    color="#E7F3FF"
  >
    <div class="d-flex flex-wrap" style="gap: 0 20px">
      <h3 class="name">
        {{ worker.full_name }}
      </h3>
      <v-chip class="pa-1 inn-badge" size="small" label
        >ИНН {{ worker.inn }}
      </v-chip>
      <div>
        <v-chip
          class="pa-1 mr-2 contract-badge bg-green"
          size="small"
          label
          color="white"
          >{{ worker.type_contract.slug }}
        </v-chip>
        <span>{{ worker.position.title }}</span>
      </div>
    </div>

    <v-row no-gutters class="worker-info mt-4 flex-wrap" style="">
      <div>
        <img
          :src=worker.county.icon
          :alt="worker.county.slug"
          class="mr-2 country-flag"
        />
        <span>{{ worker.county.slug }}</span>
      </div>
      <v-divider vertical></v-divider>
      <span>{{ worker.address }}</span>
      <v-divider vertical></v-divider>
      <span>Дата рождения: {{ worker.date_birth }}</span>
      <v-divider vertical></v-divider>
      <span>Возраст: {{ worker.age }}</span>
      <v-divider vertical></v-divider>
      <span>Пол: {{ worker.gender.title }}</span>
    </v-row>

    <v-chip
      :color="worker.status.tag.color"
      class="status-badge pa-1 mt-4"
      size="small"
      label
      ><span class="text-white">{{ worker.status.description }}</span>
    </v-chip>
  </v-card>
</template>

<script setup>
const props = defineProps({
  worker: Object,
});
</script>

<style scoped>
.worker-card {
  /* width: 100%; */
  min-height: 150px;
  box-shadow: none;
}
.name {
  font-size: 18px;
  line-height: 120%;
  color: #2a358c;
}
.worker-info {
  font-size: 14px;
  line-height: 130%;
  gap: 5px 15px;
}
.inn-badge {
  color: #84909b;
  background-color: white;
  font-weight: 500;
  font-size: 13px;
  line-height: 16px;
}
.contract-badge {
  font-weight: 600;
  font-size: 13px;
  line-height: 16px;
}
.country-flag {
  width: 20px;
  height: 14px;
  border-radius: 2px;
}
.status-badge {
  background-color: currentColor;
  font-weight: 500;
  font-size: 14px;
  line-height: 120%;
}
</style>
