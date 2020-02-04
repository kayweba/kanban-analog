<template>
  <div class="container">
    <div>
      <!-- Блок с карточкой ON HOLD -->
      <div class="card">
        <div class="title on-hold">
          <p>ON HOLD ({{ data.onHoldTasks.length }})</p>
        </div>

        <draggable
          id="first"
          data-source="juju"
          :list="data.onHoldTasks"
          class="list-group"
          draggable=".item"
          group="a"
          @change="updateDraggable"
        >
          <OnHoldItem
            @deleteOnHoldTask="doneOnHold(index)"
            :data="data"
            v-for="(data, index) in data.onHoldTasks"
            :key="index"
          ></OnHoldItem>
        </draggable>

        <div v-show="onHoldisVisible && showOnHold">
          <textarea v-model="newOnHoldTask.title" placeholder="Новая задача" />
          <div class="buttons">
            <div>
              <button @click="addOnHoldTasks" class="add-btn">Добавить карточку</button>
            </div>
            <div>
              <button class="close" @click="onHoldisVisible = !onHoldisVisible">&#10006;</button>
            </div>
          </div>
        </div>
        <button
          class="open-textarea"
          v-show="!onHoldisVisible"
          @click="onHoldisVisible = !onHoldisVisible"
        >&#10010; Добавить карточку</button>
      </div>
    </div>

    <!-- Блок с карточкой IN PROGRESS -->
    <div>
      <div class="card">
        <div class="title in-progress">
          <p>IN PROGRESS ({{ data.inProgressTasks.length }})</p>
        </div>

        <draggable
          :list="data.inProgressTasks"
          class="list-group"
          draggable=".item"
          group="a"
          @change="updateDraggable"
        >
          <InProgressItem
            class=".item"
            @deleteInProgressTask="doneInProgress(index)"
            :data="data"
            v-for="(data, index) in data.inProgressTasks"
            :key="index"
          ></InProgressItem>
        </draggable>

        <div v-show="inProgressVisible && inProgressShow">
          <textarea v-model="newInProgressTask.title" placeholder="Новая задача" />
          <div class="buttons">
            <div>
              <button @click="addInProgressTasks" class="add-btn">Добавить карточку</button>
            </div>
            <div>
              <button class="close" @click="inProgressVisible = !inProgressVisible">&#10006;</button>
            </div>
          </div>
        </div>
        <button
          class="open-textarea"
          v-show="!inProgressVisible"
          @click="inProgressVisible = !inProgressVisible"
        >&#10010; Добавить карточку</button>
      </div>
    </div>

    <!-- Блок с карточкой NEEDS REVIEW -->
    <div>
      <div class="card">
        <div class="title needs-review">
          <p>NEEDS REVIEW ({{ data.needsReviewTasks.length }})</p>
        </div>

        <draggable
          :list="data.needsReviewTasks"
          class="list-group"
          draggable=".item"
          group="a"
          @change="updateDraggable"
        >
          <NeedsReviewItem
            @deleteNeedsReviewTask="doneNeedsReview(index)"
            :data="data"
            v-for="(data, index) in data.needsReviewTasks"
            :key="index"
          ></NeedsReviewItem>
        </draggable>

        <div v-show="needsReviewIsVisible && needsReviewShow">
          <textarea v-model="newNeedsReviewTask.title" placeholder="Новая задача" />
          <div class="buttons">
            <div>
              <button @click="addNeedsReviewTask" class="add-btn">Добавить карточку</button>
            </div>
            <div>
              <button class="close" @click="needsReviewIsVisible = !needsReviewIsVisible">&#10006;</button>
            </div>
          </div>
        </div>
        <button
          class="open-textarea"
          v-show="!needsReviewIsVisible"
          @click="needsReviewIsVisible = !needsReviewIsVisible"
        >&#10010; Добавить карточку</button>
      </div>
    </div>

    <!-- Блок с карточкой APPROVED -->
    <div>
      <div class="card">
        <div class="title approved">
          <p>APPROVED ({{ data.approvedTasks.length }})</p>
        </div>

        <draggable
          :list="data.approvedTasks"
          class="list-group"
          draggable=".item"
          group="a"
          @change="updateDraggable"
        >
          <ApprovedItem
            @deleteApprovedTask="doneApproved(index)"
            :data="data"
            v-for="(data, index) in data.approvedTasks"
            :key="index"
          ></ApprovedItem>
        </draggable>

        <div v-show="approvedIsVisible && approvedShow">
          <textarea v-model="newApprovedTask.title" placeholder="Новая задача" />
          <div class="buttons">
            <div>
              <button @click="addApprovedTask" class="add-btn">Добавить карточку</button>
            </div>
            <div>
              <button class="close" @click="approvedIsVisible = !approvedIsVisible">&#10006;</button>
            </div>
          </div>
        </div>
        <button
          class="open-textarea"
          v-show="!approvedIsVisible"
          @click="approvedIsVisible = !approvedIsVisible"
        >&#10010; Добавить карточку</button>
      </div>
    </div>
  </div>
</template>



<script>
import draggable from "vuedraggable";

import OnHoldItem from "./OnHoldItem";
import InProgressItem from "./InProgressItem";
import NeedsReviewItem from "./NeedsReviewItem";
import ApprovedItem from "./ApprovedItem";

export default {
  data() {
    return {
      // Массивы с новыми задачами

      newOnHoldTask: {},
      newInProgressTask: {},
      newNeedsReviewTask: {},
      newApprovedTask: {},

      // Массивы с существующими задачами
      data: {
        onHoldTasks: [],
        inProgressTasks: [],
        needsReviewTasks: [],
        approvedTasks: []
      },

      // Показать/Скрыть форму добавления задачи
      onHoldisVisible: false,
      showOnHold: true,

      inProgressVisible: false,
      inProgressShow: true,

      needsReviewIsVisible: false,
      needsReviewShow: true,

      approvedIsVisible: false,
      approvedShow: true
    };
  },
  props: ["uid"],

  methods: {
    // Методы для карточки ON HOLD
    doneOnHold(id) {
      this.data.onHoldTasks.splice(id, 1);
      firebase
        .database()
        .ref("users/" + this.uid + "/data")
        .set({
          onHoldTasks: this.data.onHoldTasks
        });
    },

    addOnHoldTasks() {
      if (this.newOnHoldTask.title) {
        this.data.onHoldTasks.push({
          title: this.newOnHoldTask.title
        });
        this.newOnHoldTask.title = "";
      }
      firebase
        .database()
        .ref("users/" + this.uid)
        .set({
          data: this.data
        });
    },

    // Методы для карточки IN PROGRESS
    doneInProgress(id) {
      this.data.inProgressTasks.splice(id, 1);
      firebase
        .database()
        .ref("users/" + this.uid + "/data")
        .set({
          onHoldTasks: this.data.onHoldTasks
        });
    },

    addInProgressTasks() {
      if (this.newInProgressTask.title) {
        this.data.inProgressTasks.push({
          title: this.newInProgressTask.title
        });
        this.newInProgressTask.title = "";
      }
      firebase
        .database()
        .ref("users/" + this.uid)
        .set({
          data: this.data
        });
    },

    // Методы для карточки NEEDS REVIEW
    doneNeedsReview(id) {
      this.data.needsReviewTasks.splice(id, 1);
      firebase
        .database()
        .ref("users/" + this.uid + "/data")
        .set({
          onHoldTasks: this.data.onHoldTasks
        });
    },
    addNeedsReviewTask() {
      if (this.newNeedsReviewTask.title) {
        this.data.needsReviewTasks.push({
          title: this.newNeedsReviewTask.title
        });
        this.newNeedsReviewTask.title = "";
      }
      firebase
        .database()
        .ref("users/" + this.uid)
        .set({
          data: this.data
        });
    },

    // Методы для карточки APPROVED
    doneApproved(id) {
      this.data.approvedTasks.splice(id, 1);
      firebase
        .database()
        .ref("users/" + this.uid + "/data")
        .set({
          onHoldTasks: this.data.onHoldTasks
        });
    },
    addApprovedTask() {
      if (this.newApprovedTask.title) {
        this.data.approvedTasks.push({
          title: this.newApprovedTask.title
        });
        this.newApprovedTask.title = "";
      }
      firebase
        .database()
        .ref("users/" + this.uid)
        .set({
          data: this.data
        });
    },

    updateDraggable() {
      firebase
        .database()
        .ref("users/" + this.uid)
        .set({
          data: this.data
        });
    }
  },

  components: {
    OnHoldItem: OnHoldItem,
    InProgressItem: InProgressItem,
    NeedsReviewItem: NeedsReviewItem,
    ApprovedItem: ApprovedItem,
    draggable: draggable
  },

  created() {
    console.log("C R E A T E D");

    const takeOnHoldTasks = firebase
      .database()
      .ref("users/" + this.uid + "/data");

    takeOnHoldTasks.on("value", snapshot => {
      if (snapshot.val().onHoldTasks) {
        this.data.onHoldTasks = snapshot.val().onHoldTasks;
      } else {
        this.data.onHoldTasks = [];
      }
      if (snapshot.val().inProgressTasks) {
        this.data.inProgressTasks = snapshot.val().inProgressTasks;
      } else {
        this.data.inProgressTasks = [];
      }
      if (snapshot.val().needsReviewTasks) {
        this.data.needsReviewTasks = snapshot.val().needsReviewTasks;
      } else {
        this.data.needsReviewTasks = [];
      }

      if (snapshot.val().approvedTasks) {
        this.data.approvedTasks = snapshot.val().approvedTasks;
      } else {
        this.data.approvedTasks = [];
      }

      // this.data.needsReviewTasks = snapshot.val().needsReviewTasks;
      // this.data.approvedTasks = snapshot.val().approvedTasks;
    });
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
.card {
  background: #2f3137;
  width: 304px;
  margin: 20px;
}

/* Изменение стилей шапки карточек */

.title {
  color: #ffffff;
  font-size: 20px;
  height: 40px;
  display: flex;
  align-items: center;
  padding-left: 10px;
}

.on-hold {
  background: #fb7e46;
}

.in-progress {
  background: #3b8bbd;
}

.needs-review {
  background: #f4c951;
}

.approved {
  background: #4ba467;
}

.card .open-textarea {
  font-size: 16px;
  color: #76777e;
  font-weight: 500;
  padding-bottom: 10px;
  cursor: pointer;
  background: transparent;
  border: 0;
  outline: none;
}

textarea {
  transition: all 0.2s linear;
  outline: none;
  resize: none;
  box-sizing: border-box;
  padding: 10px;
  top: 0;
  border: 0;
  background: #505050;
  height: 140px;
  width: 285px;
  margin: 9px;
  color: #ffffff;
  font-size: 18px;
  left: 0;
}

.card .add-btn {
  font-weight: 400;
  outline: none;
  text-align: center;
  margin-top: 20px;
  margin-bottom: 20px;
  margin-left: 7px;
  background: #616062;
  border: 0;
  color: #ffffff;
  width: 200px;
  height: 32px;
  font-size: 16px;
}

.card .add-btn:hover {
  opacity: 0.6;
  transition: all 0.3s linear;
}

.buttons {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.close {
  padding-left: 15px;
  outline: none;
  color: #ffffff;
  margin-right: 130px;
  font-weight: medium;
  font-size: 20px;
  border: none;
  background: transparent;
}

.close:hover {
  opacity: 0.5;
  transition: all 0.2s linear;
}

.open-textarea {
  margin: 10px 0px 0px 10px;
}

.open-textarea:hover {
  opacity: 0.5;
  transition: all 0.2s linear;
}

span {
  cursor: pointer;
  color: #898b91;
  margin-left: 10px;
  margin-top: auto;
  font-size: 20px;
}

/* Анимация */
</style>

<style>
.title {
  margin-bottom: 10px;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active до версии 2.1.8 */ {
  opacity: 0;
}

/* Сброс стандартных стилей */
</style>
