<template>
  <ol class="main-list">
    <li class="main-list-wrapper">
      <div class="checkbox-group">
        <div class="checkbox-item">
          <input
              type="checkbox"
              :id="checkbox.id"
              :checked="checkbox.checked"
              :value="checkbox.value"
              @input="(event) => $emit('change-main', {id: checkbox.id, event: event.target.checked})"
          >
          <label
              :for="checkbox.id"
              class="checkmark"
          ></label>
        </div>
        <div class="arrow-right"
             :class="{rotate: checkbox.expand}"
             @click="$emit('expand', !checkbox.expand)"
        ></div>
        <span class="checkbox-label">{{ checkbox.name }}</span>
      </div>
      <ol v-if="checkbox.expand">
        <TheCheckbox
            v-for="(checkbox, idx) in checkbox.experience"
            :key="idx"
            :checked="checkbox.checked"
            :id="checkbox.id"
            :label="checkbox.name"
            :value="checkbox.value"
            @update:checked="checkedNested(checkbox.value, $event)"
        />
      </ol>
    </li>
  </ol>
</template>

<script setup>
import {defineProps, defineEmits, watch, ref} from 'vue';
import TheCheckbox from '@/components/TheCheckbox.vue';

const emit = defineEmits(
    [
      'expand',
      'change-nested',
      'change-main',
      'change-watcher',
      'update:checked',
      'update:value'
    ])

const props = defineProps({
  checkbox: {
    type: Object,
    required: true,
    default: () => {
      return {
        id: 0,
        name: '',
        checked: false,
        expand: false,
        experience: []
      }
    }
  },
  width: {
    type: String,
    required: true,
    default: ''
  },
  value: {
    type: Array,
  }
})

const arrValues = ref([])

const checkedNested = (value, event) => {
  let updatedValue = [...props.value];
  if (event) {
    updatedValue.push(value);
  } else {
    updatedValue.splice(updatedValue.indexOf(value), 1);
  }
  emit('update:value', updatedValue);

  const {checkbox} = props
  const checkedValue = checkbox.experience.map(el => {
    if (el.value === value) {
      return {...el, checked: event}
    }
    return {...el}
  })
  emit('change-nested', {id: checkbox.id, experience: checkedValue})
}

const cloneDeep = (obj) => {
  return JSON.parse(JSON.stringify(obj))
}

watch(() => cloneDeep(props.checkbox), (currentValue) => {
  if (currentValue.experience.every(el => el.checked === true)) {
    emit('change-watcher', {id: currentValue.id, event: true, type: 'watcher'})
  } else if (arrValues.value.length !== currentValue.experience.length) {
    emit('change-watcher', {id: currentValue.id, event: false, type: 'watcher'})
  }
});
</script>

<style lang="scss" scoped>
.main-list {
  width: v-bind(width);
  border-top: 1px solid rgba($second-color, 30%);
  border-right: 1px solid rgba($second-color, 30%);
  border-left: 1px solid rgba($second-color, 30%);

  &:last-of-type {
    border-bottom: 1px solid rgba($second-color, 30%);
  }

  &-wrapper {
    list-style-type: none;
  }
}

.checkbox-group {
  padding: 1rem;
  display: flex;
  align-items: center;
  background-color: $third-color;

  .checkbox-item {
    position: relative;
    width: 1.25rem;
    height: 1.25rem;
    display: flex;

    input[type="checkbox"] {
      visibility: hidden;
    }

    input[type="checkbox"]:checked + label {
      background-color: $fourth-color;
      border-color: $fourth-color;
    }

    input[type="checkbox"]:checked + label:after {
      opacity: 1;
    }


    .checkmark {
      background-color: rgba($second-color, 30%);
      border: 1px solid rgba($second-color, 30%);
      cursor: pointer;
      width: 1.25rem;
      height: 1.25rem;
      top: 0;
      left: 0;
      position: absolute;

      &:after {
        border: 2px solid $second-color;
        border-top: none;
        border-right: none;
        content: "";
        width: 12px;
        height: 6px;
        top: 0.1rem;
        left: 0.1rem;
        opacity: 0;
        position: absolute;
        transform: rotate(-45deg);

      }
    }
  }

  .arrow-right {
    margin-left: 0.75rem;
    width: 0;
    height: 0;
    border: 6px solid;
    border-color: $fourth-color $fourth-color transparent transparent;
    transform: rotate(45deg);
    cursor: pointer;
    transition: 0.5s all ease;
  }

  .rotate {
    transform: rotate(135deg);
  }

  .checkbox-label {
    margin-left: 1rem;
    line-height: 1rem;
    font-size: 1rem;
    color: $second-color;
    font-weight: 500;
  }
}
</style>