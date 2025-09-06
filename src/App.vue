<template>
  <div class="card bg-base-300 rounded-box grid h-20 place-items-center">
    <h1>TO do list</h1>
  </div>

  <form
    action=""
    @submit.prevent="addTache"
    class="flex flex-col items-center m-5"
  >
    <fieldset
      class="fieldset bg-base-200 border-base-300 rounded-box w-xs border p-4 flex"
    >
      <input
        type="text"
        class="input"
        placeholder="My awesome page"
        v-model="newTache"
      />

      <button type="submit" class="btn btn-accent">Ajouter</button>
    </fieldset>
  </form>

  <div class="flex flex-col items-center">
    <select class="select select-accent mb-5" @change="fliter" v-model="filterValue">
      <option selected value="all">Toutes</option>
      <option value="in progress">En cours</option>
      <option value="finished">Terminées</option>
    </select>
    <ul>
      <li v-for="tache in filtredTaches" :class="{ check: tache.check }">
        <Tache :item="tache" v-model="tache.check"  @delete="deleteTache" />
      </li>
    </ul>
  </div>
</template>

<script setup>
import { computed, ref, watch } from "vue";
import Tache from "./components/tache.vue";

const taches = ref(JSON.parse(localStorage.getItem("taches")) || []);

const newTache = ref("");
const filterValue = ref("all")
const filtredTaches = ref(taches.value)

watch(taches.value, () => {
  localStorage.setItem("taches", JSON.stringify(taches.value));
  const savedValue =  filterValue.value
  filterValue.value = 'intermediare'
  setTimeout(() => {
    filterValue.value = savedValue
  },5)
});

watch(filterValue,() => {
  switch(filterValue.value) {
    case "in progress": 
      filtredTaches.value = taches.value.filter(t => t.check === false)
      break;
    case "finished": 
      filtredTaches.value = taches.value.filter(t => t.check === true)
      break;
    case "intermediare":
      break
    default: 
      filtredTaches.value = taches.value
  }
})


const addTache = () => {
  if (!newTache.value) {
    alert("Veulliez entrer le nom de la tache");
  } else {
    taches.value.push({
      name: newTache.value,
      check: false,
      date: Math.floor(Date.now() / 1000),
    });

    newTache.value = "";
  }
};

const deleteTache = (id) => {
  const index = taches.value.findIndex(t => t.date === id)
  taches.value.splice(index,1)
};
</script>
<style>
.check {
  color: gray;
  text-decoration: line-through;
}
</style>
