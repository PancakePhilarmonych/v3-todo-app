<template>
  <div class="inputs">
    <button
      ref="name-button"
      @click="showListInput"
    >
      {{ value }}
    </button>

    <input
      class="hidden"
      type="text"
      ref="name-input"
      v-model="value"
      @blur="showListButton"
      @keyup.enter="showListButton"
    >
  </div>
</template>

<script>
export default {
  emits: ['update:modelValue'],

  props: {
    modelValue: {
      type: String,
      default: ''
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
  },

  methods: {
    showListInput() {
      this.$refs['name-input'].classList.remove('hidden')
      this.$refs['name-input'].focus()

      this.$refs['name-button'].classList.add('hidden')
    },

    showListButton() {
      this.$refs['name-input'].classList.add('hidden')
      this.$refs['name-button'].classList.remove('hidden')
    },
  }
}
</script>

<style scoped lang="scss">
.inputs {
  width: 90%;

  button {
    width: 100%;
    text-align: left;
    cursor: pointer;
    min-height: 24px;
    background: transparent;
    border: none;
    color: white;
    font-size: 18px;
    margin: 0;
    padding: 0;
  }

  input {
    height: 24px;
    background: transparent;
    border: none;
    padding: 0;
    color: white;
    font-size: 18px;
  }
}
</style>
