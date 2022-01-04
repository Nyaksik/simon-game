<template>
  <div class="app">
    <div class="app__game">
      <h3>{{ isFlash ? 'Запоминай последовательность' : isGame ? 'Повтори последовательность' : 'Начни игру!' }}</h3>
      <h3>Раунд {{ !isGame ? '1' : round}}</h3>
      <GameField @create="setUserColor" :block="isGame" :flash="isFlash" :color="currentColor" :fields="gameFields" />  
    </div>
    <div class="app__game-settings">
      <button @click="isGame = !isGame">{{ isGame ? 'Стоп' : 'Старт' }}</button>
      <h3>Уровень сложности:</h3>
      <input name="mode" value="1500" type="radio" v-model="mode">Лёгкий
      <input name="mode" value="1000" type="radio" checked v-model="mode">Средний
      <input name="mode" value="400" type="radio" v-model="mode">Сложный
    </div>
  </div>
</template>

<script>
  import GameField from "@/components/GameField"

  export default {
    components: {
      GameField
    },
    data() {
      return {
        gameFields: [
          {
            id: 1,
            color: "blue"
          },
          {
            id: 2,
            color: "red"
          },
          {
            id: 3,
            color: "yellow"
          },
          {
            id: 4,
            color: "green"
          }
        ],
        mode: 1000,
        isGame: false,
        isFlash: false,
        randomColor: [],
        playerColor: [],
        round: 1,
        currentColor: ''
      }
    },
    methods: {
      setUserColor(color) {
        this.playerColor = [...this.playerColor, color]
      },
      setRandomColor() {
        const colorsCount = 4
        let count = this.round
        while(count) {
          const randomDigit = Math.floor(Math.random() * colorsCount)
          this.randomColor = [...this.randomColor, this.gameFields[randomDigit].color]
          count -= 1
        }
      },
      timeout(ms) {
        return new Promise(resolve => setTimeout(resolve, ms))
      },
      async flashColor() {
        const audio = new Audio()
        this.isFlash = true    
        for(let i = 0; i < this.randomColor.length; i++) {
            await this.timeout(Number(this.mode))
            this.currentColor = this.randomColor[i]
            audio.src = `assets/${this.currentColor}.mp3`
            audio.play()
            await this.timeout(Number(this.mode))
            this.currentColor = ''
          }
        this.isFlash = false  
        }
      },
      watch: {
        isGame() {
          this.playerColor = []
          this.randomColor = []
          this.currentColor = ''
          if(this.isGame) {
            this.round = 1
            this.setRandomColor()
            this.flashColor()
          }
        },
        round() {
          if(this.isGame && this.round > 1) {
            this.playerColor = []
            this.randomColor = []
            this.currentColor = ''
            this.setRandomColor()
            this.flashColor()
          } else {
            this.round = 1
          }
        },
        playerColor(newValue) {
          for(let i = 0; i < newValue.length; i++) {
            if(newValue[i] !== this.randomColor[i]) {
              this.isGame = false
              alert('Вы проиграли!')
            }
          }
          if(newValue.length === this.randomColor.length) {
            this.round += 1
          }
        }
      }
  }
</script>

<style lang="scss">
  .app {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: nowrap;
    &__game {
      margin-right: 100px;
      text-align: center;
    }
    &__game-settings {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      & input {
          margin-top: 15px;
      }
    }
  }
</style>