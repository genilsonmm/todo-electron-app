<template>
  <div id="app">
    <div class="top-container">
      <input
        v-model="newActivity"
        ref="activity"
        v-on:keyup.enter="onEnter"
        type="text"
        placeholder="Nova atividade"
      />
    </div>
    <div class="itens-container">
      <div v-for="n in activities" :key="n.name">
        <label class="activity-item">
          {{ n.name }}
          <input type="radio" v-on:change="openDialog(n.name)" name="radio" />
          <span class="checkmark"></span>
        </label>
      </div>
    </div>
    <!-- The Modal -->
    <div id="myModal" class="modal_item" :style="computedModalStyle">
      <!-- Modal content -->
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <p>
          Remover a atividade
          <span>
            <b>{{ currentAct }}?</b>
          </span>
        </p>
        <button @click="canRemove" class="btn btn-danger float-right">Remover</button>
      </div>
    </div>
    <div class="footer-container">
      <p>{{history}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data: () => ({
    newActivity: "",
    history: "",
    currentAct: "",
    activities: [],
    modalStyle: [{ display: "none" }],
    canRemoveItem: false,
  }),
  beforeMount() {
    this.loadActivities();
  },
  mounted() {
    this.$refs.activity.focus();
    this.history = `Nehuma atividade foi concluída hoje ${new Date().toLocaleString()}`;
  },
  computed: {
    computedModalStyle() {
      return this.modalStyle;
    },
  },
  methods: {
    loadActivities() {
      let todolistItens = localStorage.getItem("todolist");

      if (todolistItens != null) {
        this.activities = JSON.parse(todolistItens);
      }
    },
    onEnter() {
      if (this.newActivity != "") {
        let act = {
          name: this.newActivity,
          done: false,
          todoDate: new Date().toLocaleString(),
          doneDate: "",
        };

        this.activities.push(act);
        localStorage.setItem("todolist", JSON.stringify(this.activities));
        this.history = `Atividade: "${
          this.newActivity
        }" ADICIONADA às ${new Date().toLocaleTimeString()}`;
        this.newActivity = "";
      }
    },
    openDialog(act) {
      this.modalStyle = [{ display: "block" }];
      this.currentAct = act;
    },
    canRemove() {
      this.$refs.activity.focus();
      this.activities = this.activities.filter(
        (item) => item.name != this.currentAct
      );
      localStorage.setItem("todolist", JSON.stringify(this.activities));
      this.history = `Atividade: "${
        this.currentAct
      }" REMOVIDA às ${new Date().toLocaleTimeString()}`;
      this.currentAct = "";
      this.modalStyle = [{ display: "none" }];
    },
    closeModal() {
      this.modalStyle = [{ display: "none" }];
      this.currentAct = "";
      location.reload()
    },
  },
};
</script>

<style>
::-webkit-scrollbar {
  display: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-color: #3f3c54;
  padding: 10px;
  -webkit-app-region: drag;

  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}
.top-container {
  align-items: flex-start;
}

.itens-container {
  height: 100%;
  max-height: 100%;
  /* background-color: aqua; */
  overflow-y: auto;
}
.footer-container {
  align-items: flex-end;
  /* background-color: #ff0000;  */
}

#app input {
  outline: none;
  margin: auto;
  width: 100%;
  text-align: center;
  color: wheat;
  font-size: 28px;
  background-color: #3f3c54;
  -webkit-app-region: no-drag;
  border-style: hidden;
}

.footer-container p {
  color: wheat;
  font-size: 10px;
  text-align: center;
}

/* The container */
.activity-item {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  color: wheat;
  -webkit-app-region: no-drag;
}

/* The Modal (background) */
.modal_item {
  display: none; /* Hidden by default */
  /* position: fixed; Stay in place */
  z-index: 1; /* Sit on top */
  /*padding-top: 100px;  Location of the box */
  left: 0;
  bottom: 0px;
  margin: auto;
  width: 80%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  -webkit-app-region: no-drag;
}

/* Modal Content */
.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 0px solid #888;
  width: 100%;
  -webkit-app-region: no-drag;
}

/* The Close Button */
.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}

/* Radio style */
.activity-item input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

/* Create a custom radio button */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: wheat;
  border-radius: 50%;
}

/* On mouse-over, add a grey background color */
.activity-item:hover input ~ .checkmark {
  background-color: wheat;
}

/* When the radio button is checked, add a blue background */
.activity-item input:checked ~ .checkmark {
  background-color: #53595e;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the indicator (dot/circle) when checked */
.activity-item input:checked ~ .checkmark:after {
  display: block;
}

/* Style the indicator (dot/circle) */
.activity-item .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #3f3c54;
}
</style>
