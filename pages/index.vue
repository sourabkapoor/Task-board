<template class>
  <div class="cards-container">
    <h3>Task Board</h3>

    <div class="taskGrpsContr">
      <!-- task groups -->
      <div v-for='(task, index) in taskGrp' :key="index">
        <TaskGroup 
          :grpInfo="task" 
          :grpTasksCount="JSON.parse(task.tasks.length)" 
          :updateItems="updateInfo"
          :taskInfo="updateTask"
        />
      </div>

      <!-- Add a new task group -->
      <AddTaskGroup :newGrpAdded="addTaskGrp"/>
    </div>
  </div>
</template>

<script>
import TaskGroup from "@/components/TaskGroup"
import AddTaskGroup from "@/components/AddTaskGroup"

export default {
  name: 'IndexPage',
  components: {
    TaskGroup,
    AddTaskGroup
  },
  data() {
    return { 
      taskGrp: [],
    };
  },
  created() {
    this.storage();
  },
  mounted() {
    // To create initila layout.
    var taskGrpArrys = [
      {title: "No status", grpId: 1, tasks: [{title: "Card 1", taskId: 1, parentGrp: 1}] }, 
      {title: "In progress", grpId: 2, tasks: [{title: "New card 1", taskId: 1, parentGrp: 2}] }
    ]
    localStorage.setItem("grpsName", JSON.stringify(taskGrpArrys))
  },
  methods: {
    storage(){
      if (process.browser){
        this.taskGrp = JSON.parse(localStorage.getItem("grpsName"))
      }
    },
    // Function to add a new task group
    addTaskGrp() {
      var newItem = {title: localStorage.getItem("newGrpName"), grpId: this.taskGrp.length + 1, tasks: []}
      this.taskGrp.push(newItem)
      localStorage.setItem("newGrpName", null);
    },
    updateInfo(dragItem, droptarget) {
      var dropGrpId = droptarget.grpId - 1
      // remove dragged item
      this.taskGrp[dragItem.parentGrp - 1].tasks.splice(dragItem.taskId - 1, 1)
      // Reorder the index
      this.taskGrp[dragItem.parentGrp - 1].tasks.forEach((task, index) => {
        task.taskId = index + 1
      });

      // Add new element to dropped array
      this.taskGrp[dropGrpId].tasks.push({title: dragItem.title, taskId: this.taskGrp[dropGrpId].tasks.length + 1, parentGrp: droptarget.grpId})
      // Reorder the index
      this.taskGrp[dropGrpId].tasks.forEach((task, index) => {
        task.taskId = index + 1
      });
    },
    updateTask(shouldDelete, taskInfo) {
      if(shouldDelete) {
        // remove that task from list
        this.taskGrp[taskInfo.parentGrp - 1].tasks.splice(taskInfo.taskId - 1, 1)
        // Reorder the index
        this.taskGrp[taskInfo.parentGrp - 1].tasks.forEach((task, index) => {
          task.taskId = index + 1
        });
      }
      else {

      }
    }
  },
}
</script>

<style lang="scss" scoped>
  .cards-container {
    padding: 30px;
    overflow-x: auto;

    h3 {
      border-bottom: 1px solid #c1c0be;
      padding-bottom: 10px;
    }
  }

  .taskGrpsContr {
    display: flex;
    align-items: flex-start;
  }
</style>
