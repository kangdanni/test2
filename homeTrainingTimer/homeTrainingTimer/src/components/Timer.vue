/* eslint-disable */
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
    <div>{{ timerFormat }}</div>
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
    <div v-show="this.mode == 1">
      <div>
        <div class="timer_loop1">
          <div class="timer" :class="{ blinking: isBlink }">
            운동 {{ seconds }}
          </div>
        </div>
      </div>
      <div>
        <div class="timer_loop2" v-show="this.leftCounter > 1">
          <div class="timer">휴식 {{ breakSeconds }}</div>
        </div>
      </div>
    </div>
    <div v-show="this.mode == 2">
      <div>
        <div class="timer_loop1">
          <div class="timer" :class="{ blinking: isBlink }">
            휴식 {{ breakSeconds }}
          </div>
        </div>
      </div>
      <div class="timer_loop2">
        <div class="timer">운동 {{ seconds }}</div>
      </div>
    </div>

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
          <button v-if="timerBtn == 0" @click="wholeTimerLoop()">
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
      timeCounter: 10,
      defaultTimeCounter: 10,
      timerBtn: 0, // 0 :start  1: pause
      isBlink: false,
      //isBreak: false,
      timerON: "",
      // counter: 10,
      sCount: "",
      setTime: "",
      round: 3,
      cycle: 3,
      isTurn: true,
      defaultRound: 3,
      defaultCycle: 3,
      btnTxt: "시작",
      healthMode: true,
      breakCount: "",
      breakTimeCounter: 5,
      defaultBreakTimeCounter: 5,
      breakTimerON: "",
      totalTime: "",
      defaultTotalTime: "",
      mode: 1,
      leftCounter: 0,
      defaultLeftCount: 0
    };
  },

  methods: {
    editTime(obj) {
      this.round = obj.inputRound;
      this.cycle = obj.inputCycle;
      this.timeCounter = obj.inputTime;
      this.breakTimeCounter = obj.inputBreak;

      this.defaultTimeCounter = this.timeCounter;
      this.defaultCycle = this.cycle;
      this.defaultRound = this.round;
      this.defaultBreakTimeCounter = this.breakTimeCounter;

      this.leftCount;

      this.setTotalAmt;
    },

    healthTimerLoop() {
      console.log("healthtimer");
      if (this.count < this.defaultTimeCounter) {
        this.countTimer();
        if (this.timeCounter <= 3) {
          this.isBlink = true;
        }
      } else {
        this.leftCounter--;
        this.round--;
        if (this.round === 0) {
          this.cycle--;
        }
        clearTimeout(this.timerOn);
        this.countRefresh();
        this.mode = 2;
        this.countTimer();
      }
    },

    breakTimerLoop() {
      console.log("breaktimer");
      if (this.breakCount < this.defaultBreakTimeCounter) {
        this.countTimer();
        if (this.breakTimeCounter <= 3) {
          this.isBlink = true;
        }
      } else {
        this.isBlink = false;
        this.leftCounter--;
        //this.cycle--;
        clearTimeout(this.breakTimerON);
        this.mode = 1;
        this.countRefresh();

        setTimeout(this.wholeTimerLoop, 1000);
      }
    },
    countTimer() {
      //Cycle 내 Round가 끝날 때 바로 Round초기화해줌
      if (this.round === 0 && this.cycle > 0) {
        this.roundRefresh();
      }

      //운동모드
      if (this.mode === 1) {
        this.count++;
        this.timeCounter = this.timeCounter - 1;
        this.totalTime--;
        this.timerOn = setTimeout(this.healthTimerLoop, 1000);
      }
      //휴식모드
      if (this.mode === 2) {
        this.breakCount++;
        this.breakTimeCounter = this.breakTimeCounter - 1;
        this.totalTime--;
        if (this.leftCounter > 1) {
          this.breakTimerON = setTimeout(this.breakTimerLoop, 1000);
        } else {
          alert("운동끝");

          this.initTimer();
          // this.refresh();
          // this.leftCounter = this.defaultLeftCount;
          // this.roundRefresh();
          // this.cycleRefresh();
        }
      }
    },

    wholeTimerLoop() {
      console.log("wholetimer");

      this.timerBtn = 1;
      if (this.leftCounter > 0) {
        this.countTimer();
      }
    },
    countTime() {
      if (this.healthMode) {
        this.count++;
        this.timeCounter = this.timeCounter - 1;
        this.timerOn = setTimeout(this.timerLoop, 1000);
      } else {
        if (this.cycle > 0) {
          this.breakCount++;
          this.breakTimeCounter = this.breakTimeCounter - 1;
          this.breakTImerON = setTimeout(this.test, 1000);
        } else {
          clearTimeout(this.breakTImerON);
        }
      }
    },

    test() {
      if (this.breakTimeCounter === 0) {
        this.healthMode = true;
        this.breakTimeCounter = this.defaultBreakTimeCounter;
      }
      this.countTime();
    },

    timerLoop() {
      this.timerBtn = 1;

      if (this.count < this.defaultTimeCounter) {
        this.countTime();
        if (this.timeCounter <= 3) {
          this.isBlink = true;
        }
      } else {
        //운동타이머가 끝나면 휴식시간이 시작된다.
        this.healthMode = false;

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
            console.log("운동 끝!");
          }
          //alert("timeout");
          // this.isBreak = true;
          // this.refresh();
        }
      }
    },

    // pause() {
    //   clearTimeout(this.timerOn);
    //   this.timerBtn = 0;
    // },
    pause() {
      this.timerBtn = 0;
      if (this.mode === 1) {
        console.log("운동멈춰");
        clearTimeout(this.timerOn);
      } else {
        console.log("breaktime멈춰");
        clearTimeout(this.breakTimerON);
      }
    },
    roundRefresh() {
      this.round = this.defaultRound;
    },
    cycleRefresh() {
      this.cycle = this.defaultCycle;
      this.mode = 1;
    },
    countRefresh() {
      this.count = 0;
      this.isBlink = false;
      this.timeCounter = this.defaultTimeCounter;

      this.breakTimeCounter = this.defaultBreakTimeCounter;
      this.breakCount = 0;
    },
    refresh() {
      this.pause();

      this.count = 0;
      this.isBlink = false;
      this.timeCounter = this.defaultTimeCounter;

      this.breakTimeCounter = this.defaultBreakTimeCounter;
      this.breakCount = 0;

      this.totalTime = this.defaultTotalTime;
      //this.leftCounter = 3;
      //this.leftCount;
      // this.leftCounter = this.defaultLeftCount;
      this.leftCounter =
        parseInt(this.defaultRound) * parseInt(this.defaultCycle) * 2 - 1;
    },
    initTimer() {
      this.refresh();
      this.cycleRefresh();
      this.roundRefresh();

      this.healthMode = true;
    },
    closeModal() {
      this.editModal = false;
    }
  },

  created() {
    this.leftCount;
    this.setTotalAmt;
  },
  computed: {
    setTotalAmt() {
      var a =
        this.defaultTimeCounter * this.defaultRound * this.defaultCycle +
        this.defaultBreakTimeCounter *
          (this.defaultRound * this.defaultCycle - 1);
      this.totalTime = a;
      this.defaultTotalTime = a;
      // return Math.trunc(a / 60) + ":" + ("0" + (a % 60)).slice(-2);
    },

    timerFormat() {
      return (
        Math.trunc(this.totalTime / 60) +
        ":" +
        ("0" + (this.totalTime % 60)).slice(-2)
      );
    },
    leftCount() {
      console.log("leftcount");

      return (this.leftCounter =
        parseInt(this.defaultRound) * parseInt(this.defaultCycle) * 2 - 1);
    },

    classObj() {
      this.btnTxt = this.timerBtn == 1 ? "정지" : "시작";
      return this.timerBtn == 1 ? "pause_btn" : "";
    },
    seconds() {
      return this.modifiedDate;
    },
    breakSeconds() {
      return this.modifiedBreakDate;
    },
    modifiedDate: function() {
      // return Math.trunc(this.now)

      return (
        Math.trunc(this.timeCounter / 60) +
        " : " +
        ("0" + (this.timeCounter % 60)).slice(-2)
      );
    },
    modifiedBreakDate: function() {
      // return Math.trunc(this.now)

      return (
        Math.trunc(this.breakTimeCounter / 60) +
        " : " +
        ("0" + (this.breakTimeCounter % 60)).slice(-2)
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
  /*width: 100%; */
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: whitesmoke;
  /* font-family: Arial, Helvetica, sans-serif; */
  background-color: dodgerblue;
}

.timer_loop2 {
  height: 100px;
  /* width: 100%; */
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: whitesmoke;
  /* font-family: Arial, Helvetica, sans-serif; */
  background-color: gold;
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
