<template>
  <div v-if="editModal" id="editModal" class="modal">
    <div class="modal-content">
      <div>
        <div class="title">Edit Timer</div>
        <button class="close" @click="$emit('closeModal')">
          <i class="fas fa-times"></i>
        </button>
      </div>
      <div class="edit-content-box">
        time:
        <input
          v-model="editTimer.inputTime"
          style="border:1px solid"
          type="text"
        />
        round:
        <input
          v-model="editTimer.inputRound"
          style="border:1px solid"
          type="text"
        />
        cycle:
        <input
          v-model="editTimer.inputCycle"
          style="border:1px solid"
          type="text"
        />
      </div>
      <div class="submit">
        <button @click="$emit('editTime', editTimer)">submit</button>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./HelloWorld.vue";

export default {
  components: {
    HelloWorld
  },
  props: ["editModal"],
  data() {
    return {
      closeModal: false,
      editTimer: {
        inputTime: "",
        inputRound: "",
        inputCycle: ""
      }
    };
  },

  methods: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* The Modal (background) */
.modal {
  display: block;
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 40% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
  height: 50%; /* Could be more or less, depending on screen size */
}
/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}
.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.title {
  display: inline;
}
.edit-content-box {
  margin-top: 40px;
}
.edit-content-box input {
  display: inline-block;
  margin: 4px;
}

.submit {
  margin-top: 20px;
}
</style>
