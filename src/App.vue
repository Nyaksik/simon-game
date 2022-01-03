<template>
  <div class="app">
    <div class="app__game">
      <GameField @create="setUserColor" :color="currentColor" :fields="gameFields" />  
    </div>
    <div class="app__game-settings">
      <h3>Раунд {{ !isGame ? '1' : round}}</h3>
      <button @click="isGame = !isGame">{{ isGame ? 'Рестарт' : 'Старт'}}</button>
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
        let count = this.round
        while(count !== 0) {
          const randomDigit = Math.floor(Math.random() * 4)
          this.randomColor = [...this.randomColor, this.gameFields[randomDigit].color]
          count -= 1
        }
      },
      timeout(ms) {
        return new Promise(resolve => setTimeout(resolve, ms))
      },
      async flashColor() {
        const audio = new Audio()
            
        for(let i = 0; i < this.randomColor.length; i++) {
            await this.timeout(Number(this.mode))
            this.currentColor = this.randomColor[i]
            audio.src = `assets/${this.currentColor}.mp3`
            audio.play()
            await this.timeout(Number(this.mode))
            this.currentColor = ''
          }
        }
      },
      watch: {
        isGame() {
          if(this.isGame) {
            this.playerColor = []
            this.randomColor = []
            this.round = 1
            this.setRandomColor()
            this.flashColor()
          } else {
            this.randomColor = []
            this.playerColor = []
            this.currentColor = ''
          }
        },
        round(newValue, prev) {
          if(newValue - prev > 1) {
            this.round = prev + 1
          }
          if(this.isGame && this.round > 1) {
            this.randomColor = []
            this.playerColor = []
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
            if(newValue.length === this.randomColor.length) {
              this.round += 1
            }
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