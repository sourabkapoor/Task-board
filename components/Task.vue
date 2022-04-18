<template>
  <div class="task-contr" @click="toggleModal">
    <p>{{ cardInfo.title }}</p>

    <b-modal :ref="modalName" :id="modalName" 
      title="Edit task information"
      hide-footer
    >
      <div class="taskInfoContr">
        <input required class="inputText" :value="cardInfo.title" placeholder="Enter title" @change="updateTitle($event.target.value)"/> 
        <input class="inputStatus" placeholder="Status" @change="updateStatus($event.target.value)" />
        <input class="inputDesc" placeholder="Description" @change="updateDesc($event.target.value)" />

        <div class="actnBtnContr">
          <BIconTrash @click="updateInfo(true)"/>
          <div class="saveBtn" @click="updateInfo(false)">Save</div>
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>
import { BIconTrash } from "bootstrap-vue"
export default {
  components: { BIconTrash },
  props: [ "cardInfo", "deleteitem", "saveInfo" ],
  data() {
    return { 
      modalName: "taskInfoModal" + this.cardInfo.taskId + this.cardInfo.parentGrp,
      title: this.cardInfo.title,
      desc: this.cardInfo.desc,
      status: this.cardInfo.status,
    };
  },
  methods: {
    toggleModal() {
      this.$refs[this.modalName].show()
    },
    updateTitle(value) {
      this.title = value
    },
    updateStatus(value) {
      this.status = value
    },
    updateDesc(value) {
      this.desc = value
    },
    updateInfo(isDeleted) {
      if(isDeleted)
        this.saveInfo(isDeleted, this.cardInfo)
    },
  }
}
</script>


<style lang="scss" scoped>
  .task-contr {
    box-shadow: rgba(9, 30, 66, 0.25) 0px 1px 1px, rgba(9, 30, 66, 0.13) 0px 0px 1px 1px;
    border-radius: 7px;
    padding: 6px;
    margin-bottom: 8px;
    width: 220px;
    cursor: move;
  }

  p {
    margin-bottom: 0;
  }

  .taskInfoContr {
    padding: 15px;
      
    .inputText {
      font-size: 26px;
      font-weight: 600;
      border: none !important;
      margin-bottom: 8px;
      width: 100%;
    }

    .inputStatus {
      font-size: 15px;
      border: none !important;
      width: 100%;
      margin: 5px 0;
    }

    .inputDesc {
      font-size: 13px;
      border: none !important;
      width: 100%;
      margin: 5px 0 30px;
    }

    .actnBtnContr {
      display: flex;
      justify-content: flex-end;
      align-items: center;
      width: 100%;

      svg {
        font-size: 20px;
        margin-right: 20px;
        cursor: pointer;

        &:hover {
          color: red;
        }
      }

      .saveBtn {
        font-size: 17px;
        color: black;
        padding: 5px 10px;
        background: rgb(196, 196, 196);
        border-radius: 7px;
        // cursor: pointer;
        pointer-events: none;
      }
    }
  }
</style>