<template>
  <div id="app">
    <img src="./assets/cronometro.png" alt="cronomtro" class="img">

    <h3 class="timer"> {{ tempo }}</h3>

    <div class="divBtn">
      <button class="buton" @click="iniciar"><strong>{{ botao }}</strong></button>
      <button class="buton" @click="limpar"><strong>Limpar</strong></button>
    </div>

    <div class="listaPausa" v-show="historico.length > 0">
      <ul>
        <li v-for="(item, index) in historico" :key="item">{{ index+1 }}° Pausa: {{ item }}</li>
        <button class="buton" style="margin-top: 10px;" @click="historico = []"><strong>Limpar Histórico</strong></button>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      timer: null,
      tempo: '00:00,00',
      botao: 'Iniciar',
      ss: 0,
      mm :0,
      hh: 0,
      historico: []
    }
  },
  methods: {
    iniciar() {
      if(this.timer !== null) {
        clearInterval(this.timer)
        this.timer = null
        this.botao = "Iniciar"

        if(this.ss !== 0 || this.mm !== 0 || this.hh !== 0) {
          this.historico.push(this.tempo)
        }
      } else {
        this.timer = setInterval(() => {
          this.rodarTimer()
          this.botao = "Pausar"
        }, 100)
      }
    },
    rodarTimer() {
      this.ss++

      if(this.ss === 59) {
        this.ss = 0
        this.mm++
      }

      if(this.mm === 59) {
        this.mm = 0
        this.hh++
      }

      let hora = (this.hh < 10 ? '0'+this.hh : this.hh) + ":" +
      (this.mm < 10 ? '0'+this.mm : this.mm) + "," +
      (this.ss < 10 ? '0'+this.ss : this.ss)

      return this.tempo = hora
    },
    limpar() {
      clearInterval(this.timer)
      this.timer = null
      this.botao = "Iniciar"
      this.tempo = "00:00,00"
      this.historico = []
    }
  }
}
</script>

<style>
  .img {
    width: 450px;
  }

  .timer {
    margin-top: -140px;
    font-size: 26px;
    color: #000025;
  }

  .divBtn {
    margin-top: 120px;
  }

  .buton {
    font-family: 'Montserrat', sans-serif;
    margin-left: 10px;
    margin-right: 10px;
    padding: 10px;
    color: #000025;
    background: #ffff60;
    width: 100px;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    border: 0;
    border-radius: 5px;
    text-align: center;
  }

  .listaPausa ul {
    text-align: center;
    padding: 0;
  }
  .listaPausa ul li {
    color: white;
    list-style: none;
    margin-top: 8px;
  }

</style>
