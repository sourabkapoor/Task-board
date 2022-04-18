<template>
  <div style="width: 220px;" @click="createEntry()">
      <div v-if="!createMode">
        <p class="addNew" style="padding: 6px 0;">
          <BIconPlus /> New
        </p>
      </div>
      <div v-else>
        <Input :nameComplete="addGroup" inputFor="task" />
      </div>
  </div>
</template>

<script>
import { BIconPlus } from "bootstrap-vue"
import Input from "./Input.vue"
export default {
  components: {
    BIconPlus,
    Input
  },
  props: {
    newTaskAdded: {
      type: Function
    }
  },
  data() {
    return { 
      createMode: false
    };
  },
  methods: {
    // toggle between view & edit mode.
    createEntry() {
      this.createMode = !this.createMode
    },
    // Callback to add a new task group
    addGroup() {
      this.newTaskAdded()
    }
  }
}
</script>
