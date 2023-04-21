<script setup>
import { DeletedTasks } from "src/composables/Tasks";
</script>

<template>
  <div class="flex justify-center">
    <div class="full-width q-pa-xl">
      <h6 class="q-my-none q-mb-md">Deleted Tasks</h6>
      <div class="q-mt-xl">
        <q-card class="q-mt-sm" v-for="row in DeletedTasks" :key="row.id">
          <q-card-section class="bg-primary text-white q-pa-none">
            <div class="text-bold q-pa-md">{{ row.todo }}</div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </div>
</template>
