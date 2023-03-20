<template>
  <div class="template">
    <TheCheckbox
        :id="0"
        :value="'all_checkbox'"
        :label="'All selected'"
        :checked="false"
        styled
        @input="allSelect($event.target.checked)"
    ></TheCheckbox>
    <TheGroupCheckboxes
        v-for="(checkbox, idx) in checkboxes"
        :key="idx"
        :checkbox="checkbox"
        :width="'20rem'"
        @expand="isExpand(checkbox.id, $event)"
        @change-nested="changeNested($event)"
        @change-main="changeMain($event)"
        @change-watcher="changeWatcher($event)"
        v-model:value="selectedValues"
    />
  </div>
  <ol>
    <li class="list" v-for="item in selectedValues" :key="item">{{ item }}</li>
  </ol>
</template>

<script setup>
import TheGroupCheckboxes from "@/components/TheGroupCheckboxes.vue";
import {ref} from "vue";
import {checkboxData} from "@/data/data";
import TheCheckbox from "@/components/TheCheckbox.vue";

const checkboxes = ref(checkboxData)
const selectedValues = ref([])

const isExpand = (id, event) => {
  checkboxes.value = checkboxes.value.map(el => {
    if (el.id === id) {
      el.expand = event
    }
    return el
  })
}

const allSelect = (e) => {
  checkboxes.value.forEach(el => {
    if (e) {
      selectedValues.value.push(el.value)
      el.expand = true
      el.checked = true
      el.experience.forEach(item => {
        selectedValues.value.push(item.value)
        item.checked = true
      })
    } else {
      selectedValues.value = []
      el.expand = false
      el.checked = false
      el.experience.forEach(item => {
        selectedValues.value = []
        item.checked = false
      })
    }
  })
}

const changeWatcher = (e) => {
  const item = checkboxes.value.find(el => el.id === e.id)
  if (e.event) {
    return item.checked = true
  } else {
    return item.checked = false
  }
}
const changeMain = (e) => {
  const item = checkboxes.value.find(el => el.id === e.id)
  if (e.event) {
    item.checked = true
    item.expand = true
    item.experience.forEach(el => {
      selectedValues.value.push(el.value)
      el.checked = true
    })
  } else {
    item.checked = false
    item.experience.forEach(el => {
      selectedValues.value.splice(selectedValues.value.indexOf(el.value), 1);
      el.checked = false
    })
  }
}
const changeNested = (e) => {
  const item = checkboxes.value.find(el => el.id === e.id)
  item.experience = e.experience

}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300;400;500;600;700&display=swap');

* {
  font-family: 'Quicksand', sans-serif;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html, body {
  height: 100%;
  background-color: $first-color;
}

#app {
  padding-top: 6rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  background-color: $first-color;
}

.template {
  display: flex;
  flex-direction: column;
}

.list {
  display: flex;
  justify-content: center;
  width: 20rem;
  list-style-type: none;
  padding: 1rem;
  color: $second-color;
  font-size: 1.2rem;
  border-bottom: 1px solid rgba($second-color, 30%);
}
</style>
