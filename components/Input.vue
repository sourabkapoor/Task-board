<template>
  <input 
    id="taskInput" 
    class="taskInput" 
    placeholder="Type a name"
    @change="fieldName($event.target.value)"
  />
</template>

<script>
export default {
  props: {
    nameComplete: {
      type: Function
    },
    inputFor: {
      type: String
    }
  },
  mounted() {
    if(document.getElementById("taskInput"))
      document.getElementById("taskInput").focus()
  },

  methods: { 
    fieldName(text) {
      if(text !== "") {
        if(this.inputFor !== "task"){
          localStorage.setItem("newGrpName", text);
          document.getElementById("taskInput").click()
          this.nameComplete(text)
        }
        else{
          console.log("1111", text)
          localStorage.setItem("newTaskName", text)
          document.getElementById("taskInput").click()
          this.nameComplete(text)
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
 .taskInput {
   border: none;
 }
</style>