<template>
  <input
    class="card card-input"
    :placeholder="placeholder"
    v-model="value"
    type="text"
    @keyup.enter="onEnter"
  >
</template>

<script>
export default {
  props: {
    modelValue: {
      type: String,
      default: ''
    },

    placeholder: {
      type: String,
      default: ''
    }
  },
  emits: ['update:modelValue', 'onEnterHandler'],

  methods: {
    onEnter() {
      this.$emit('onEnterHandler')
    }
  },

  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  }
}
</script>

<style>

</style>