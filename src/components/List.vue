<template>
  <div class="list">
    <div class="card title">
      <div class="options">
        <item-name-input v-model="listName" />

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
          <item-name-input v-model="task.name" />

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
import { VueDraggableNext } from "vue-draggable-next";
import IconTrash from '@/components/IconTrash.vue'
import BaseInput from "@/components/BaseInput.vue";
import ItemNameInput from '@/components/ItemNameInput.vue';

export default {
  components: {
    draggable: VueDraggableNext,
    BaseInput,
    IconTrash,
    ItemNameInput
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