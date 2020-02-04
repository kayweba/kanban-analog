<template>
  <div class="item">
    <div class="card__id">
      <p>
        <span>Id: {{ 1 }}</span>
      </p>
      <p class="delete" @click="deleteNeedsReviewTask">&#10006;</p>
    </div>
    <p>{{ data.title }}</p>
  </div>
</template>
<script>
export default {
  props: ["data"],
  data() {
    return {
      tasks: [{}]
    };
  },

  methods: {
    deleteNeedsReviewTask() {
      this.$emit("deleteNeedsReviewTask");
    }
  }
};
</script>

<style scoped>
.item {
  box-sizing: border-box;
  width: 286px;
  background: #1f2027;
  margin: 9px;
}

.item span {
  color: #ffffff;
}

.item p {
  color: #76777e;
  padding: 10px;
}

.card__id {
  display: flex;
  justify-content: space-between;
  padding: 0 10px;
}
.card__id span {
  color: #ffffff;
}

.delete {
  cursor: pointer;
}

.delete:hover {
  transition: all 0.3s ease;
  color: #ffffff;
}
</style>
