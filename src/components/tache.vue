<template>
  <div class="flex justify-between">
    <div>
      <input
        type="checkbox"
        :name="item.name"
        :key="item.date"
        :id="item.dtae"
        class="checkbox checkbox-accent"
        v-model="model"
      />
      <label class="xl m-2">{{ item.name }}</label>
    </div>
    <button class="btn btn-soft btn-accent" @click="emits('delete',item.date)">SUPPRIMER</button>
  </div>
</template>

<script setup>
const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
});

const emits = defineEmits(['delete'])

const model = defineModel();
</script>
