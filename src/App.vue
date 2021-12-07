<template>
  <div class="cronoArea">
    <img src="./assets/cronometro.png" alt="Cronometro" class="img" />
    <span class="timer">{{ formattedTime }}</span>
  </div>

  <div class="btnArea">
    <button class="btn" @click="go">{{ buttonText }}</button>
    <button class="btn" @click="clear">CLEAR</button>
  </div>

  <div class="list" v-show="listHistory.length > 0">
    <ul>
      <li v-for="hist in listHistory" :key="hist">
        VOCÊ FEZ UMA PAUSA EM: {{ hist }}
      </li>
    </ul>
    <button @click="clearHistory">Limpar histórico</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      buttonText: 'GO',
      timer: null,
      ss: 0,
      mm: 0,
      hh: 0,
      listHistory: [],
    };
  },
  methods: {
    go() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
        this.buttonText = 'GO';
        this.listHistory.push(this.formattedTime);
      } else {
        this.timer = setInterval(this.initTimer, 100);
        this.buttonText = 'PAUSE';
      }
    },
    clear() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }

      this.buttonText = 'GO';
      this.clearHistory();
      this.ss = 0;
      this.mm = 0;
      this.hh = 0;
    },
    clearHistory() {
      this.listHistory = [];
    },
    initTimer() {
      this.ss++;

      if (this.ss === 59) {
        this.ss = 0;
        this.mm++;
      }

      if (this.mm === 59) {
        this.mm = 0;
        this.hh++;
      }
    },
  },
  computed: {
    formattedTime() {
      const hh = String(this.hh).padStart(2, '0');
      const mm = String(this.mm).padStart(2, '0');
      const ss = String(this.ss).padStart(2, '0');
      return `${hh}:${mm}:${ss}`;
    },
  },
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
  justify-content: center;
}

.cronoArea {
  margin-top: 100px;
  position: relative;
}

.img {
  width: 420px;
  height: 420px;
}

.timer {
  color: #fff;
  font-size: 70px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%);
}

.btnArea {
  margin-top: 25px;
  display: flex;
}

.btn {
  width: 150px;
  background-color: #fff;
  font-size: 20px;
  border: 0;
  text-align: center;
  margin: 0 15px;
  padding: 6px;
  cursor: pointer;
  user-select: none;
  transition: all 0.3s;
}

.btn:hover {
  opacity: 0.8;
}

ul {
  margin: 16px 0;
  text-align: center;
  list-style: none;
  padding: 0;
}

ul li {
  margin-top: 4px;
  padding: 15px;
  background-color: rgb(70, 70, 70);
  color: #fff;
  font-size: 18px;
  border-radius: 6px;
}

.list button {
  cursor: pointer;
  border: 0;
  background-color: #fff;
  margin-bottom: 12px;
  padding: 8px;
}
</style>
