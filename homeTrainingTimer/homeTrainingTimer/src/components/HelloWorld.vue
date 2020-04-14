<template>
  <div>
    <div class="greetings">
      헬린이 💪
    </div>
    <div>
      <input class="settimer" type="text" placeholder="set your workout time" />
    </div>
    <div class="myTimer">
      <h2>
        {{ seconds }}
      </h2>
    </div>
    <div class="timerinput">
      <button class="startBtn" @click="myTimer">{{ setTimer }}</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: "",
      timeCounter: 180, //3분
      setTimer: "start"
    };
  },
  mounted() {
    // this.myTimer();
  },

  methods: {
    myTimer() {
      this.count++;
      if (this.count < 181) {
        this.timeCounter = this.timeCounter - 1;
        setTimeout(this.myTimer, 1000);
      } else {
        this.setTimer = "reset";

        console.log("end");
      }
    }
  },

  computed: {
    seconds() {
      return this.modifiedDate;
    },

    modifiedDate: function() {
      // return Math.trunc(this.now)
      return (
        Math.trunc(this.timeCounter / 60) +
        " : " +
        ("0" + (this.timeCounter % 60)).slice(-2)
      );
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.startBtn {
  border: 1px;
  text-align: center;
  background-color: darkkhaki;
}
.settimer {
  border: 1px green;
}
</style>
