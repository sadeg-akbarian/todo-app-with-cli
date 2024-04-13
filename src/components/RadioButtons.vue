<template>
  <div class="radioButtonsContainer">
    <div class="singleRadioContainer">
      <input
        type="radio"
        id="allButton"
        name="threeRadioButtons"
        checked
        @click="changeRadioState($event), sendRadioStateToHomeView()"
      />
      <label for="allButton">All</label>
    </div>
    <div class="singleRadioContainer">
      <input
        type="radio"
        id="openButton"
        name="threeRadioButtons"
        @click="changeRadioState($event), sendRadioStateToHomeView()"
      />
      <label for="openButton">Open</label>
    </div>
    <div class="singleRadioContainer">
      <input
        type="radio"
        id="doneButton"
        name="threeRadioButtons"
        @click="changeRadioState($event), sendRadioStateToHomeView()"
      />
      <label for="doneButton">Done</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "RadioButtons",
  data() {
    return {
      activeButton: "allButton",
    };
  },
  methods: {
    changeRadioState(event) {
      this.activeButton = event.target.id;
    },
    sendRadioStateToHomeView() {
      this.$emit("currentRadioState", this.activeButton);
    },
  },
  emits: ["currentRadioState"],
};
</script>

<style scoped>
.radioButtonsContainer {
  display: flex;
  flex-direction: column;
  margin-left: 2rem;
}

.singleRadioContainer {
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
}

label {
  font-size: 1.5rem;
  font-weight: 700;
  margin-left: 0.5rem;
}
</style>
