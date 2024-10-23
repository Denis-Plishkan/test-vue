<script setup lang="ts">
interface InputProps {
  modelValue: string | number
  type?: string
  placeholder?: string
}

defineProps<InputProps>()

const emit = defineEmits(['update:modelValue'])

const updateValue = (event: Event) => {
  const target = event.target as HTMLInputElement
  emit('update:modelValue', target.value)
}
</script>

<template>
  <input
    :type="type || 'text'"
    :placeholder="placeholder"
    :value="modelValue"
    @input="updateValue"
    class="input-field"
  />
</template>

<style lang="scss">
@use 'BaseInput';
</style>
