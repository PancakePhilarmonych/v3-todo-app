<template>
  <div class="list">
    <div class="card title">
      <div class="options">
        <div class="inputs">
          <button
            :ref="`list-name-button-${list.name}`"
            @click="showListInput(`${list.name}`)"
          >
            {{ listName }}
          </button>

          <input
            class="hidden"
            type="text"
            :ref="`list-name-input-${list.name}`"
            v-model="listName"
            @blur="showListButton(`${list.name}`)"
            @keyup.enter="showListButton(`${list.name}`)"
          >
        </div>

        <div class="icon">
          <div
            class="remove"
            @click="removeList"
          >
            &#10005;
          </div>
        </div>
      </div>
    </div>

    <draggable
      :item-key="`${listIndex-taskName}`"
      :options="{ group: 'tasks'}"
      :list="list.tasks"
      group="tasks"
      ghost-class="ghost"
      class="cards"
    >
      <div
        class="card"
        v-for="(task, taskIndex) in list.tasks"
        :key="`${list.name}-${taskIndex}`"
      >
        <div class="options">
          <div class="inputs">
            <button
              :ref="`task-button-${`${taskIndex}-${list.name}`}`"
              @click="showInput(`${taskIndex}-${list.name}`)"
            >
              {{ task.name }}
            </button>

            <input
              class="hidden"
              :ref="`task-input-${`${taskIndex}-${list.name}`}`"
              type="text"
              v-model="task.name"
              name="taskName"
              @blur="showButton(`${taskIndex}-${list.name}`)"
              @keyup.enter="showButton(`${taskIndex}-${list.name}`)"
            >
          </div>

          <div class="icon">
            <div
              class="remove"
              @click="removeItem(taskIndex)"
            >
              <icon-trash />
            </div>
          </div>
        </div>
      </div>
    </draggable>

    <base-input
      v-model="taskName"
      placeholder="+ Add new task"
      @onEnterHandler="addNewTaskName"
    />
  </div>
</template>

<script>
import IconTrash from '@/components/IconTrash.vue'
import { VueDraggableNext } from "vue-draggable-next";
import BaseInput from "@/components/BaseInput.vue";

export default {
  components: {
    draggable: VueDraggableNext,
    BaseInput,
    IconTrash
  },

  emits: [
    'addNewTaskName',
    'removeItem',
    'changeListName',
    'removeList'
  ],

  props: {
    list: {
      type: Object,
      required: true,
    },

    listIndex: {
      type: Number,
      required: true
    }
  },

  data() {
    return {
      taskName: '',
      listName: '',
    }
  },

  mounted() {
    this.updateName()
  },

  methods: {
    updateName() {
      this.listName = this.list.name
    },

    removeItem(taskIndex) {
      this.$emit('removeItem', [this.listIndex, taskIndex])
    },

    removeList() {
      this.$emit('removeList')
    },

    showInput(refName) {
      this.$refs[`task-input-${refName}`].classList.remove('hidden')
      this.$refs[`task-input-${refName}`].focus()

      this.$refs[`task-button-${refName}`].classList.add('hidden')
    },

    showButton(refName) {
      this.$refs[`task-input-${refName}`].classList.remove('hidden')
      this.$refs[`task-button-${refName}`].classList.add('hidden')
    },

    showListInput(refName) {
      this.$refs[`list-name-input-${refName}`].classList.remove('hidden')
      this.$refs[`list-name-input-${refName}`].focus()

      this.$refs[`list-name-button-${refName}`].classList.add('hidden')
    },

    showListButton(refName) {
      this.$refs[`list-name-input-${refName}`].classList.remove('hidden')
      this.$refs[`list-name-button-${refName}`].classList.add('hidden')
    },

    addNewTaskName() {
      if(!this.taskName) {
        return
      }

      this.$emit('addNewTaskName', [this.taskName, this.listIndex ])
      this.taskName = ''
    }
  },

  watch: {
    listName(value) {
      this.$emit('changeListName', [value, this.listIndex])
    }
  }
}
</script>

<style scoped lang="scss">
  .list {
    border-radius: 6px;
    height: fit-content;
    background: #313641;
    flex-direction: column;
    width: 272px;
    transition: all 0.3s;
    font-size: 18px;
    padding: 12px;

    & > * {
      width: 100%;
    }

    .title {
      display: flex;
      align-items: center;
      min-height: 32px;
      word-break: break-all;
      overflow-wrap: break-word;
      margin-bottom: 12px;
    }
  }
</style>