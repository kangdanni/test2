<template>
  <div>
    <div class="greetings">
      헬린이 💪
    </div>
    <div class="timer_loop">
      <h2>
        {{ seconds }}
      </h2>
    </div>
    <div>
      <button class="startBtn" @click="timerLoop">{{ timerBtn }}</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: "",
      timeCounter: 10,
      timerBtn: "start"
    };
  },
  mounted() {
    // this.myTimer();
  },

  methods: {
    timerLoop() {
      this.count++;
      if (this.count < 11) {
        this.timeCounter = this.timeCounter - 1;
        setTimeout(this.timerLoop, 1000);
        if (this.timeCounter <= 3) {
          var elem = document.getElementsByClassName("timer_loop")[0];
          elem.style.backgroundColor = "red";
        }
      } else {
        alert("timeout");
        this.count = 0;
        this.timeCounter = 10;
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
.greetings {
  font-size: 20px;
}
.timer_loop {
  margin-top: 50px;
  border: 15px solid grey;
}

.startBtn {
  margin-top: 50px;
  border: 1px;
  text-align: center;
  background-color: darkkhaki;
}
.settimer {
  border: 1px green;
}
</style>
