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
    <div class="timer_loop1" :class="{ break_time: isBreak }">
      <!-- <div class="refresh_btn">
        <button @click="refresh">
          <i class="fas fa-sync-alt"></i>
        </button>
      </div> -->
      <div class="timer" :class="{ blinking: isBlink }">
        {{ seconds }}
      </div>
    </div>

    <div>
      <input style="border:1px solid" type="text" v-model="setTime" />
    </div>
    <div class="timer_loop2">
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
    </div>

    <div>
      <div class="bottom_btns">
        <div class="left_round">
          {{ round }}
        </div>
        <div class="play_btn">
          <button v-if="timerBtn == 0" @click="timerLoop">
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
        <div class="play_btn">
          시작
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: "",
      timeCounter: 10,
      timerBtn: 0, // 0 :start  1: pause
      isBlink: false,
      isBreak: false,
      timerON: "",
      counter: 10,
      sCount: "",
      setTime: "",
      defaultTime: "0:10",
      round: 3,
      cycle: 3,
      isTurn: true
    };
  },
  mounted() {
    // this.myTimer();
  },

  methods: {
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
      this.timeCounter = this.timeCounter - 1;
      this.timerOn = setTimeout(this.timerLoop, 1000);
    },

    timerLoop() {
      //this.isBreak = false;
      this.timerBtn = 1;
      if (this.count < 10) {
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
      this.round = 3;
    },

    refresh() {
      this.pause();
      this.count = 0;
      this.isBlink = false;
      this.timeCounter = 10;
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
  color: blueviolet;
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
  display: flex;
  justify-content: center;
  align-items: center;
  color: whitesmoke;
  font-family: Arial, Helvetica, sans-serif;
  background-color: dodgerblue;
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
  /* text-align: center; */
}

.refresh_btn {
  float: right;
  margin-left: 5px;
  font-size: 15px;
}

.refresh_btn i {
  font-size: 20px;
  z-index: -1;
  margin-top: 8px;
  color: tan;
}

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
