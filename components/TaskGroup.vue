<template class>
  <div 
    class="taskGrpContainer drop-zone" 
    :id="[`taskGrpContainer${grpInfo.grpId}`]"
    @drop="onDrop(grpInfo)"  
    @dragover.prevent
    @dragenter.prevent
  >
    <div class="TaskGrpHeader">{{ grpInfo.title }} <span>{{ grpTasksCount }}</span></div>
      <!-- tasks list -->
      <div v-for='(task, index) in taskList' :key="index">
        <div class="drag-Item" draggable="true" @dragstart="startDrag($event, task)" :key="taskList + index">
          <Task 
            :cardInfo="task" 
            :saveInfo="saveTaskInfo" 
          />
        </div>
      </div>

      <!-- Add task button -->
      <AddTask :newTaskAdded="addTask"/>
  </div>
</template>


<script>
import Task from "./Task.vue"
import { BIconPlus } from "bootstrap-vue"
import AddTask from "./AddTask"
var dragged = null;

export default {
  props: {
    grpInfo: {
      type: Object
    },
    grpTasksCount: {
      type: Number
    },
    updateItems: {
      type: Function
    },
    taskInfo: {
      type: Function
    }
  },
  components: {
    Task,
    BIconPlus,
    AddTask
  },
  data() {
    return { 
      taskList: this.grpInfo.tasks,
    };
  },
  methods: {
    addTask() {
      this.taskList.push({title: localStorage.getItem("newTaskName"), taskId: this.grpInfo.tasks.length + 1, parentGrp: this.grpInfo.grpId})
    },
    saveTaskInfo(isDeleted, taskInfo) {
      this.taskInfo(isDeleted, taskInfo)
    },
    startDrag (evt, item) {
      evt.dataTransfer.dropEffect = 'move'
      evt.dataTransfer.effectAllowed = 'move'
      dragged = item
  	},
    onDrop (dropEle) {
      this.updateItems(dragged, dropEle)
  	}
  }
}
</script>

<style lang="scss">
  .taskGrpContainer {
    margin: 1rem;
    max-width: max-content;
  }

  .TaskGrpHeader {
    padding: 0 5px;
    font-size: 13px;
    margin-bottom: 8px;
    color: black;

    span {
      color: #c1c0be;
      margin-left: 3px;
      font-size: 10px;
    }
  }

  .addNew {
    color: #c1c0be;
    cursor: pointer;
    display: flex;
    align-items: center;
    padding: 6px 12px;
    border-radius: 7px;
    width: 220px;

    svg {
      font-size: 25px;
    }

    &:hover {
      background: rgb(236, 236, 236);
    }
  }
</style>