<script setup>
import { ref } from "vue";
import { MyTasks, FinishedTasks } from "src/composables/Tasks";

let text = ref(null);
let form = ref(null); // Element ref for q-form
let selectedTodo = ref(null); // State that stores the current selected row
let toMarkAsDone = ref(null); // State that stores the row that would be marked as done

// addTodo
// This function pushes an item to the MyTasks list
// and resets the value of the input field
const addTodo = () => {
  let id = MyTasks.value.length;
  MyTasks.value.push({
    id: ++id,
    todo: text.value,
  });

  text.value = null;
  form.value.reset();
};

// selectTodo
// Function that sets the state of selectedTodo
const selectTodo = (row) => {
  selectedTodo.value = row;
  text.value = row.todo;
};

// updateTodo
// This function finds the id of selected row from the list
// and updates the value. This also resets the input field.
const updateTodo = () => {
  let index = MyTasks.value.findIndex((t) => t.id === selectedTodo.value.id);
  index !== -1 && (MyTasks.value[index].todo = text.value);

  text.value = selectedTodo.value = null;
  form.value.reset();
};

// toggleDialog
// This function toggles the confirmation dialog
let showDialog = ref(false);
const toggleDialog = (row) => {
  showDialog.value = true;
  toMarkAsDone.value = null;
  toMarkAsDone.value = row;
};

// markAsDone
// This function looks at toMarkAsDone and finds the same id inside the
// MyTasks list and removes it when found. This then adds it to the
// FinishedTasks list.
const markAsDone = () => {
  let index = MyTasks.value.findIndex((t) => t.id === toMarkAsDone.value.id);
  index !== -1 && MyTasks.value.splice(index, 1);
  FinishedTasks.value.push(toMarkAsDone.value);
  toMarkAsDone.value = null;
  showDialog.value = false;
};
</script>

<template>
  <div class="flex justify-center">
    <div class="full-width q-pa-xl">
      <h6 class="q-my-none q-mb-md">My Tasks</h6>
      <!-- If there's selectedTodo, run updateTodo function -->
      <q-form @submit="!selectedTodo ? addTodo() : updateTodo()" ref="form">
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
            <!-- Conditionally render button -->
            <q-btn
              :class="`bg-${!selectedTodo ? 'primary' : 'secondary'} fit`"
              color="white"
              glossy
              dense
              flat
              type="submit"
              :icon="!selectedTodo ? 'add' : 'edit'"
            />
          </div>
        </div>
      </q-form>
      <div class="q-mt-xl">
        <!-- Render each item in MyTasks list -->
        <q-card
          class="q-mt-sm"
          v-for="row in MyTasks"
          :key="row.id"
          @click="selectTodo(row)"
        >
          <!-- Current selected todo row state -->
          <q-card-section
            :class="`bg-${
              selectedTodo && selectedTodo.id === row.id
                ? 'secondary'
                : 'primary'
            } text-white q-pa-none flex justify-between items-center`"
          >
            <div class="text-bold q-pl-lg">{{ row.todo }}</div>
            <div class="bg-white q-pa-sm">
              <q-btn
                @click.stop="toggleDialog(row)"
                flat
                icon="check_circle_outline"
                color="green"
              />
              <q-btn flat icon="delete_outline" color="red" />
            </div>
          </q-card-section>
        </q-card>

        <q-dialog v-model="showDialog" persistent>
          <q-card>
            <q-card-section class="row items-center">
              <div class="q-ml-sm">
                Are you sure you want to mark
                <span class="text-green">"{{ toMarkAsDone.todo }}"</span> as
                done?
              </div>
            </q-card-section>

            <q-card-actions align="right">
              <q-btn flat label="Cancel" color="primary" v-close-popup />
              <q-btn
                @click="markAsDone()"
                flat
                label="Yes"
                color="primary"
                v-close-popup
              />
            </q-card-actions>
          </q-card>
        </q-dialog>
      </div>
    </div>
  </div>
</template>
