<template>
  <li class="items">
    <input type="checkbox" v-model="item.checked" />
    <div>{{ item.id }}.</div>
    <div
      class="items_div"
      :class="{ comp: item.completed }"
      @click="$emit('completed')"
    >
      {{ item.todo }}
    </div>
    <button type="inline" class="trash" @click="$emit('remove')">
      <img src="@/assets/trash.svg" alt="" />
    </button>
    <button type="inline" class="trash" @click="$emit('edit')">
      <img src="@/assets/pen.svg" alt="" />
    </button>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      default: () => {}
    }
  }
};
</script>

<style scoped>
.items_div {
  background: #ffffff;
  box-shadow: 0px 10px 30px rgba(138, 149, 158, 0.1);
  border-radius: 16px;
  padding: 15px 20px;
  cursor: pointer;
  width: 600px;
}
.items {
  display: flex;
  gap: 16px;
  align-items: center;
  font-weight: 500;
  letter-spacing: -0.3px;
  margin-bottom: 20px;
}
.trash {
  width: 30px;
  height: 30px;
  padding: 5px 0;
  border-radius: 50%;
  display: inline-block;
  border: none;
  background: #f6f6f6;
}
</style>
