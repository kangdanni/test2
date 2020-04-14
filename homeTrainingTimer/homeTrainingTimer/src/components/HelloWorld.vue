<template>
  <div class="hello">
    {{ seconds }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: "",
      timeCounter: 180 //3분
    };
  },
  mounted() {
    this.myTimer();
  },

  methods: {
    myTimer() {
      this.count++;
      if (this.count < 181) {
        this.timeCounter = this.timeCounter - 1;
        setTimeout(this.myTimer, 1000);
      } else {
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
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
