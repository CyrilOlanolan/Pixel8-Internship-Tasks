<script setup>
import { ref } from "vue";
import { MyTasks } from "src/composables/Tasks";

let text = ref(null);
</script>

<template>
  <div class="flex justify-center">
    <div class="full-width q-pa-xl">
      <h6 class="q-my-none q-mb-md">My Tasks</h6>
      <q-form>
        <div class="row gap">
          <q-input
            dense
            outlined
            label="What's your todo?"
            class="q-pa-none col-10"
            lazy-rules
            :rules="[(val) => !!val || 'Field is required']"
            v-model="text"
          />
          <div class="col-2 q-pl-sm">
            <q-btn
              class="bg-primary fit"
              color="white"
              glossy
              dense
              flat
              type="submit"
              icon="add"
            />
          </div>
        </div>
      </q-form>
      <div class="q-mt-xl">
        <q-card class="q-mt-sm">
          <q-card-section
            class="bg-primary text-white q-pa-none flex justify-between items-center"
          >
            <div class="text-bold q-pl-lg">Todo</div>
            <div class="bg-white q-pa-sm">
              <q-btn flat icon="check_circle_outline" color="green" />
              <q-btn flat icon="delete_outline" color="red" />
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </div>
</template>
