<template>
  <div style="margin: 1rem; width: 220px;" @click="createEntry()">
      <div v-if="!createMode">
        <p class="addNew">
          <BIconPlus /> Add a group
        </p>
      </div>
      <div v-else>
        <Input :nameComplete="addGroup" inputFor="taskGroup" />
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
    newGrpAdded: {
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
      this.newGrpAdded()
    }
  }
}
</script>
