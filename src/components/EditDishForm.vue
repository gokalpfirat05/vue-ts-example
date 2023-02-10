<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { v4 as uuidv4 } from 'uuid'
import { useDishStore } from '@/stores/DishStore';
import type { Dish } from '@/types';

const props = defineProps<{
    dishId: string
}>();

const dishStore = useDishStore();

const emit = defineEmits<{
  (e: 'add-new-dish', dish: Dish): void,
  (e: 'cancel-edit-dish'): void
}>();

const targetDish = dishStore.getDishById(props.dishId);

const elNameInput = ref<HTMLInputElement | null>(null);

const newDish = ref<Dish>({
  id: uuidv4(),
  name: '',
  status: 'Want to Try',
  diet: '',
});

const updateDish = () => {
    targetDish.name = newDish.value.name;
}

const addNewDish = () => {
  emit('add-new-dish', newDish.value);
};

const cancelNewDish = () => {
  emit('cancel-edit-dish');
};

onMounted(() => {
  elNameInput.value?.focus();
})
</script>

<template>
  <form @submit.prevent>
    <div class="field">
      <div class="field">
        <label for="name" class="label">Edit Name {{ dishId }}</label>
        <div class="control">
          <input
            :value="newDish.name"
            @keyup.enter="updateDish"
            type="text"
            class="input is-large"
            placeholder="Mystery Flavored Shrimp"
            required
            ref="elNameInput"
          />
        </div>
      </div>
      <div class="field">
        <div class="buttons">
          <button @click="addNewDish" class="button is-success">Create</button>
          <button @click="cancelNewDish" class="button is-light">Cancel</button>
        </div>
      </div>
    </div>
  </form>
</template>

<style></style>
