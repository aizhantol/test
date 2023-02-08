<template>
  <div>
    <div class="search" :style="{ width: width }">
      <input
        v-model="text"
        name="search"
        class="search__inp"
        placeholder="Поиск"
      />
      <button width="100px" class="search__btn" @click="$emit('search', text)">
        <img src="@/assets/search.svg" alt="" />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: ""
    };
  }
};
</script>

<style scoped>
.search {
  display: flex;
  flex-direction: row;
  border-radius: 26px;
  background: #f6f6f6;
  height: 50px;
  margin-bottom: 20px;
  overflow: hidden;
  max-width: 800px;
}
.search__btn {
  border-radius: 0 26px 26px 0 !important;
}
.search__inp {
  width: calc(100% - 100px);
}
input {
  height: 50px;
  background: none;
  outline: none;

  border: 1px solid transparent;
  border-radius: 26px 0 0 26px !important;
}
</style>
