<template>
  <div>
    <link
      rel="stylesheet"
      href="https://use.fontawesome.com/releases/v5.13.0/css/all.css"
    />
    <link
      rel="stylesheet"
      href="https://use.fontawesome.com/releases/v5.13.0/css/v4-shims.css"
    />
    <div class="greetings">
      헬린이 💪
    </div>

    <div class="upper-side-btns">
      <div class="edit_btn">
        <button @click="editModal = true">
          <i class="far fa-edit"></i>
        </button>
      </div>

      <div class="refresh_btn">
        <button @click="initTimer">
          <i class="fas fa-sync-alt"></i>
        </button>
      </div>
    </div>
    <div>
      <div class="timer_loop1" :class="{ break_time: isBreak }">
        <div class="timer" :class="{ blinking: isBlink }">
          {{ seconds }}
        </div>
      </div>
    </div>

    <!-- <div>
      <input style="border:1px solid" type="text" v-model="timeCounter" />
    </div> -->
    <!-- <div class="timer_loop2">
      <div>
        🙇‍♀️
      </div>
      <div class="refresh_btn">
        <button @click="timerInterval()">
          <i class="far fa-play-circle"></i>
        </button>
      </div>
      <div id="demo">
        {{ defaultTime }}
      </div>
    </div> -->

    <div>
      <div class="bottom_btns">
        <div class="left_round">
          {{ round }}
        </div>
        <div class="play_btn" :class="classObj">
          <button v-if="timerBtn == 0" @click="timerLoop(setTime)">
            <i class="far fa-play-circle"></i>
          </button>
          <button v-if="timerBtn == 1" @click="pause">
            <i class="fas fa-pause-circle"></i>
          </button>
        </div>

        <div class="left_cycle">
          {{ cycle }}
        </div>
      </div>

      <div class="bottom_btns">
        <div class="left_round">
          round
        </div>
        <div class="play_btn" :class="classObj">
          {{ btnTxt }}
        </div>
        <div class="left_cycle">
          cycle
        </div>
      </div>

      <!-- <div class="start_btn">
        <button v-if="timerBtn == 0" class="startBtn" @click="timerLoop">
          <i class="far fa-play-circle"></i>
          Start
        </button>
        <button v-if="timerBtn == 1" class="startBtn" @click="pause">
          <i class="far fa-pause-circle"></i>
          Stop
        </button>
      </div> -->
    </div>
    <edit-modal
      :editModal="editModal"
      @closeModal="closeModal"
      @editTime="editTime"
    ></edit-modal>
  </div>
</template>

<script>
import EditModal from "./EditModal.vue";
export default {
  components: {
    EditModal
  },
  //props: ["editTimer"],
  data() {
    return {
      editModal: false,
      count: "",
      timeCounter: 30,
      defaultTimeCounter: 30,
      timerBtn: 0, // 0 :start  1: pause
      isBlink: false,
      isBreak: false,
      timerON: "",
      // counter: 10,
      sCount: "",
      setTime: "",
      round: 3,
      cycle: 3,
      isTurn: true,
      defaultRound: 3,
      defaultCycle: 3,
      btnTxt: "시작"
    };
  },
  mounted() {
    // this.myTimer();
  },

  methods: {
    editTime(obj) {
      this.round = obj.inputRound;
      this.cycle = obj.inputCycle;
      this.timeCounter = obj.inputTime;

      this.defaultTimeCounter = this.timeCounter;
      this.defaultCycle = this.cycle;
      this.defaultRound = this.round;
      console.log("kkkcscs");
    },
    timerInterval() {
      if (!this.setTime) {
        var time = 10;
      } else {
        var time = this.setTime;
      }

      var min = "";
      var sec = "";

      var x = setInterval(() => {
        min = parseInt(time / 60);
        sec = ("0" + (time % 60)).slice(-2);
        document.getElementById("demo").innerHTML = min + ":" + sec;
        time--;
        console.log(time);
        if (time < 0) {
          this.round--;

          clearInterval(x);
          document.getElementById("demo").innerHTML = "end";
        }
      }, 1000);
    },

    timerIntervalLoop() {
      console.log("kkk몰라");
    },

    countTime() {
      this.count++;
      console.log("kkkk");
      this.timeCounter = this.timeCounter - 1;
      this.timerOn = setTimeout(this.timerLoop, 1000);
    },

    timerLoop() {
      this.timerBtn = 1;

      if (this.count < this.defaultTimeCounter) {
        this.countTime();
        if (this.timeCounter <= 3) {
          this.isBlink = true;
        }
      } else {
        if (this.round >= 1) {
          this.round--;
          this.refresh();
          this.timerLoop();
        }
        if (this.round == 0) {
          if (this.cycle > 0) {
            this.cycle--;
            this.refresh();
            this.roundRefresh();
            this.timerLoop();
          }

          if (this.cycle == 0) {
            this.refresh();
            this.roundRefresh();
            this.cycleRefresh();
            alert("운동 끝!");
          }
          //alert("timeout");
          // this.isBreak = true;
          // this.refresh();
        }
      }
    },

    pause() {
      clearTimeout(this.timerOn);
      this.timerBtn = 0;
    },

    roundRefresh() {
      this.round = this.defaultRound;
    },
    cycleRefresh() {
      this.cycle = this.defaultCycle;
    },
    refresh() {
      this.pause();
      this.count = 0;
      this.isBlink = false;
      this.timeCounter = this.defaultTimeCounter;
    },
    initTimer() {
      this.refresh();
      this.cycle = this.defaultCycle;
      this.round = this.defaultRound;
    },
    closeModal() {
      this.editModal = false;
    }
  },

  computed: {
    classObj() {
      this.btnTxt = this.timerBtn == 1 ? "정지" : "시작";
      return this.timerBtn == 1 ? "pause_btn" : "";
    },
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
.bottom_btns {
  width: 100%;
  margin: 10px auto;
  display: flex;
}
.left_round {
  flex: 1;
  width: 30%;
  box-sizing: border-box;
}

.play_btn {
  flex: 1;
  margin: 0px 5%;
  width: 30%;
  box-sizing: border-box;
  color: dodgerblue;
}
.left_cycle {
  flex: 1;
  width: 30%;
  box-sizing: border-box;
}

.greetings {
  font-size: 20px;
}
.timer_loop1 {
  height: 200px;
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: whitesmoke;
  font-family: Arial, Helvetica, sans-serif;
  background-color: dodgerblue;
}

.upper-side-btns {
  display: flex;
  flex-direction: row-reverse;
}
.edit_btn {
  margin: 3px 10px;
}
.refresh_btn {
  margin: 4px 20px 3px 3px;
  font-size: 15px;
}

.break_time {
  background-color: gray;
  height: 100px;
}

.startBtn {
  margin-top: 50px;
  border: 1px;
  text-align: center;
  color: dodgerblue;
  font-weight: bold;
  font-size: 20px;
  background-color: white;
}

.timer {
  font-size: 70px;
  font-weight: bold;
  display: inline-block;
  /* text-align: center; */
}
.pause_btn {
  color: grey;
}

/* .refresh_btn i {
  font-size: 20px;
  z-index: -1;
  margin-top: 8px;
  color: tan;
} */

.blinking {
  -webkit-animation: blink 0.5s ease-in-out infinite alternate;
  -moz-animation: blink 0.5s ease-in-out infinite alternate;
  animation: blink 0.5s ease-in-out infinite alternate;
  color: red;
}
@-webkit-keyframes blink {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@-moz-keyframes blink {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
@keyframes blink {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
