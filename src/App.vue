<template>
  <div class="container">
    <draggable
      class="list-draggable"
      :list="lists"
      :options="{ group: 'lists' }"
      group="lists"
      ghost-class="ghost"
      @change="updateListNames"
    >
      <list
        v-for="(list, listIndex) in lists"
        :ref="`list-${listIndex}`"
        :list-index="listIndex"
        :key="`key-${listIndex}-${getGeneratedId()}`"
        :list="list"
        :style="setlistColorVariable(list.color)"
        @addNewTaskName="addNewTaskName"
        @changeListName="changeListName"
        @removeItem="removeItem"
        @removeList="removeList(listIndex)"
      />
    </draggable>

    <base-input
      v-model="newListName"
      placeholder="+ Add new list"
      @onEnterHandler="addNewList"
    />
  </div>
</template>

<script>
import colors from '../src/services/colors.js'
import { VueDraggableNext } from 'vue-draggable-next';
import BaseInput from "@/components/BaseInput.vue";
import List from '@/components/List.vue';

export default {
  name: 'App',

  components: {
    List,
    BaseInput,
    draggable: VueDraggableNext,
  },

  mounted() {
    this.lists = JSON.parse(localStorage.getItem('list')) || []
  },

  data() {
    return {
      newListName: '',
      newTaskName: '',

      lists: [],
    }
  },

  computed: {
    colorsKeys() {
      return Object.keys(colors)
    }
  },

  methods: {
    updateListNames() {
      this.lists.forEach((list, index) => {
        this.$refs[`list-${index}`].updateName()
      })
    },

    changeListName(args) {
      this.lists[args[1]].name = args[0]
    },

    removeItem(args) {
      this.lists[args[0]].tasks.splice(args[1], 1);
      this.updateListNames()
    },

    removeList(index) {
      this.lists.splice(index, 1);
    },

    addNewList() {
      if(!this.newListName) {
        return
      }

      if(this.lists.find(list => list.name === this.newListName)) {
        return
      }

      this.lists.push({
        name: this.newListName,
        tasks: [],
        color: this.getRandomColor()
      })

      this.newListName = ''
    },

    addNewTaskName(args) {
      this.lists[args[1]].tasks.push({name: args[0]})
    },

    randomColorIndex() {
      return Math.round(Math.random() * this.colorsKeys.length)
    },

    getRandomColor() {
      return colors[this.colorsKeys[this.randomColorIndex()]]
    },

    setlistColorVariable(color) {
      return {'--list-color': color}
    },

    getGeneratedId() {
      return '_' + Math.random().toString(36).substr(2, 9);
    },
  },

  watch: {
    lists: {
      handler(val) {
        localStorage.setItem('list', JSON.stringify(val))
      },
      deep: true
    },
  }
}
</script>

<style  lang="scss">


.cards > * {
  background: var(--list-color);
  margin-bottom: 10px;
}

.list-draggable {
  display: flex;
  gap: 10px;
}

.hidden {
  display: none;
}
</style>
