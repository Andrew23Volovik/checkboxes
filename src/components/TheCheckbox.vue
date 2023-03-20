<template>
  <li class="nested-list-wrapper" :class="{styled: styled}">
    <div class="checkbox-wrapper">
      <div class="checkbox-item">
        <input
            type="checkbox"
            :id="id"
            :checked="checked"
            :value="value"
            @input="(event) => $emit('update:checked', event.target.checked)"
        >
        <label
            :for="id"
            class="checkmark"
        ></label>
      </div>
      <span class="checkbox-label">{{ label }}</span>
    </div>
  </li>
</template>

<script setup>
import {defineProps, defineEmits} from "vue";

defineEmits(['update:checked'])

defineProps({
  id: {
    type: Number,
    required: true,
    default: 0
  },
  checked: {
    type: Boolean,
    required: true,
    default: false
  },
  label: {
    type: String,
    required: true,
    default: ''
  },
  value: {
    type: String,
    required: true,
    default: ''
  },
  styled: {
    type: Boolean,
    required: false,
    default: false
  },
})
</script>

<style lang="scss" scoped>
.nested-list-wrapper {
  list-style-type: none;
  padding: 0.75rem;
  border-bottom: 1px solid rgba($second-color, 30%);
  background-color: $third-color;

  &:first-child {
    border-top: 1px solid rgba($second-color, 30%);
  }

  &:last-child {
    border-bottom: none;
  }
}

.styled {
  border-bottom: none;
  background-color: $first-color;

  &:first-child {
    border-top: none;
  }
}

.checkbox-wrapper {
  display: flex;
  align-items: center;

  .checkbox-item {
    margin-left: 1.5rem;
    position: relative;
    width: 1.25rem;
    height: 1.25rem;

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

  .checkbox-label {
    margin-left: 1rem;
    line-height: 1rem;
    font-size: 1rem;
    color: $second-color;
  }
}
</style>