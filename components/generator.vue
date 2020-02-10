<template>

  <div class="container">
    <div id="progress" :style="{width:progress}"></div>
    <div id="tst" :class="{'show-final' :showFinal}">
      {{genratorSteps[0].value}}
      <!-- <temp name= {{ genratorSteps[1].value }} ></temp> -->
      </div>
    <div id="generate">
      <i v-if="position===0" class="previousButton fa fa-user"></i>
      <i v-else class="previousButton fa fa-arrow-left" @click="previousStep"></i>
      <i class="forwardButton fa fa-arrow-right" @click="checkStep"></i>
      <div id="inputContainer" :class="{'showContainer':showContainer}">
        <form @submit.prevent="checkStep">
          <input
            id="inputField"
            type="inputType"
            v-model="inputValue"
            ref="generatorinput"
            required
            @input="changemessage"
          />
          <label id="inputLabel">{{ inputLabel }}</label>
        </form>
        <div id="inputProgress"></div>
      </div>
    </div>
  </div>
</template>
<script>
// import temp from "./Temp.vue";

export default {
  name: "HelloWorld",
  // components:{
  //   temp
  // },
  data() {
    return {
      position: 0,
      inputLabel: "",
      inputType: "text",
      inputValue: " ",
      showContainer: false,
      shoeFinal: false,
      progress: "0%",
      genratorSteps: [
        {
          label: "what's your first name?",
          type: "text",
          value: "",
          pattern: /.+/
        },
        {
          label: "what's your last name?",
          type: "text",
          value: "",
          pattern: /.+/
        },
        {
          label: "what's your email?",
          type: "email",
          value: "",
          pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
        },
        {
          label: "what's your address?",
          type: "address",
          value: "",
          pattern: /^[^\s@]+-[^\s@]+-[^\s@]+$/
        },
        {
          label: "Profail",
          type: "text",
          value: "",
          pattern: /.+/
        }
      ]
    };
  },
  methods: {
    setStep() {
      this.inputLabel = this.genratorSteps[this.position].label;
      this.inputType = this.genratorSteps[this.position].type;
      this.inputValue = this.genratorSteps[this.position].value;
      this.$refs.generatorinput.focus();
      this.showStep();
    },
    showStep() {
      setTimeout(() => {
        this.showContainer = true;
      }, 100);
    },
    hideStep(callback) {
      this.showContainer = false;
      setTimeout(callback, 100);
    },
    previousStep() {
      let comp1 = document.getElementById("generate");
      this.position -= 1;
      comp1.className = "";
      this.hideStep(this.setStep);
      this.setProgress();
    },
    checkStep() {
      if (!this.genratorSteps[this.position].pattern.test(this.inputValue)) {
        let comp1 = document.getElementById("generate");

        comp1.classList.add("wrong");
        comp1.classList.add("wronganimation");
        setTimeout(() => {
          comp1.classList.remove("wronganimation");
        }, 500);
        this.$refs.generatorinput.focus();
      } else {
        let comp1 = document.getElementById("generate");
        comp1.className = "";
        comp1.classList.add("okanimation");
        setTimeout(() => {
          comp1.classList.remove("okanimation");
        }, 200);

        this.genratorSteps[this.position].value = this.inputValue;
        this.position++;

        if (this.genratorSteps[this.position]) {
          this.hideStep(this.setStep);
        } else {
          this.hideStep(() => {
            comp1.className = "close";
            setTimeout(() => {
              this.showFinal = true;
            }, 800);
          });
        }
      }
      this.setProgress();
    },
    setProgress() {
      this.progress = (this.position / this.genratorSteps.length) * 100 + "%";
    }
  },

  mounted() {
    //let comp1 = document.getElementById("generate");

    this.setStep();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import url(//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css);

.container {
  position: relative;
  font-family: "Noto Sans", sans-serif;
  font-size: 1rem;
  color: #333;
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: radial-gradient(#673dba, #5420ff);

  #progress {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background-color: #5420ff;
    transition: width 0.8s ease-in-out;
  }
}

#tst {
  position: absolute;
  width: max-content;
  font-size: 2rem;
  color: #fff;
  opacity: 0;
  transition: 0.8s ease-in-out;
  &.show-final {
    opacity: 1;
  }
}

#generate {
  position: relative;
  width: 480px;
  height: 80px;
  padding: 10px;
  background-color: #fff;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);

  &.close {
    width: 0;
    padding: 10px 0;
    overflow: hidden;
    transition: 0.8s ease-in-out;
    box-shadow: 0 16px 24px 2px rgba(0, 0, 0, 0.2);
  }
}

.previousButton {
  position: absolute;
  left: 30px;
  top: 12px;
  font-size: 1rem;
  color: #9e9e9e;
  cursor: pointer;
  z-index: 20;
  &:hover {
    color: #673dba;
  }
}

.forwardButton {
  position: absolute;
  top: 30px;
  right: 20px;
  font-size: 3rem;
  color: #5420ff;
  cursor: pointer;
  z-index: 20;
  &:hover {
    color: #673dba;
  }
}
#inputContainer {
  position: relative;
  padding: 30px 20px 20px 20px;
  margin-right: 60px;
  opacity: 0;
  transition: opacity 0.3s ease-in-out;
  input {
    position: relative;
    width: 100%;
    font-size: 1.35rem;
    font-weight: bold;
    outline: 0;
    background: transparent;
    box-shadow: none;
    border: none;
    &:valid + #inputLabel {
      top: 3px;
      left: 43px;
      font-size: 1rem;
    }
  }
}

.wrong .forwardButton {
  color: #d93f38;
}
.close .forwardButton,
.close .previousButton {
  color: #fff;
}
#inputLabel {
  position: absolute;
  top: 35px;
  left: 20px;
  font-size: 1.35rem;
  font-weight: bold;
  pointer-events: none;
  transition: 0.2s ease-in-out;
}
#inputProgress {
  width: 0%;
  border-bottom: 6px solid #5420ff;
  transition: width 0.6s ease-in-out;
}

.wrong #inputProgress {
  border-color: #d93f38;
}
.showContainer {
  opacity: 1 !important;

  #inputProgress {
    width: 100%;
  }
}
.wronganimation {
  animation: 0.5s linear 1 worng-animation;
}
.okanimation {
  animation: 0.2s linear 1 ok-animation;
}

@keyframes worng-animation {
  0% {
    transform: translateX(0);
  }
  20% {
    transform: translateX(-20px);
  }
  40% {
    transform: translateX(20px);
  }
  60% {
    transform: translateX(-20px);
  }
  80% {
    transform: translateX(20px);
  }
  100% {
    transform: translateX(0);
  }
}
@keyframes ok-animation {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(10px);
  }
  100% {
    transform: translateY(0px);
  }
}
</style>
