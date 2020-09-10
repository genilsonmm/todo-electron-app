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
          <input type="radio" v-on:change="remove(n.name)" name="radio" />
          <span class="checkmark"></span>
        </label>
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
    activities: [],
  }),
  beforeMount() {
   
    let todolistItens = localStorage.getItem("todolist");

    if (todolistItens != null) {
      this.activities = JSON.parse(todolistItens);
    }

    //alert(screen.width)
  },
  mounted() {
    this.$refs.activity.focus();
    this.history = `Nehuma atividade foi concluída hoje ${new Date().toLocaleString()}`;
  },
  methods: {
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
    remove(act) {
      this.$refs.activity.focus();
      this.activities = this.activities.filter((item) => item.name != act);
      localStorage.setItem("todolist", JSON.stringify(this.activities));
      this.history = `Atividade: "${act}" REMOVIDA às ${new Date().toLocaleTimeString()}`;
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
