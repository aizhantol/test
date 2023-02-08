<template>
  <div class="switch">
    <label
      v-for="name in names"
      :key="name"
      :class="[value == name && 'active']"
    >
      {{ name }}
      <input
        :id="name"
        v-model="model"
        name="switch"
        type="radio"
        :value="name"
      />
    </label>
  </div>
</template>

<script>
export default {
  model: {
    prop: "value",
    event: "change"
  },
  data() {
    return {
      value: "все",
      names: ["выполненные", "не выполненные", "все"]
    };
  },
  computed: {
    model: {
      get() {
        return this.value;
      },
      set(newValue) {
        this.value = newValue;
        this.$emit("change", newValue);
      }
    }
  }
};
</script>

<style scoped>
.switch {
  display: flex;
  margin-bottom: 20px;
}
label {
  font-size: 16px;
  line-height: 19px;
  letter-spacing: -0.3px;
  color: #00c48c;
  display: block;
  text-align: center;
  padding: 10px 0;
  border: 1px solid #00c48c;
  width: 175px;
  cursor: pointer;
}

label:last-child {
  border-radius: 0 10px 10px 0;
}
label:first-child {
  border-radius: 10px 0 0 10px;
}
label > input {
  width: 0;
  height: 0;
  position: absolute;
  opacity: 0;
  visibility: hidden;
}
.active {
  color: white;
  background: #00c48c;
}
</style>
