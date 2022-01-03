<template>
  <div class="app">
    <div class="app__game">
      <GameField @create="setUserColor" v-model="playerColor" :game="isGame" :color="currentColor" :fields="gameFields" />  
    </div>
    <div class="app__game-settings">
      {{round}}
      <button @click="isGame = !isGame">{{ isGame ? 'Рестарт' : 'Старт'}}</button>
      <input name="mode" value="1500" type="radio" v-model="mode">1.5s
      <input name="mode" value="1000" type="radio" checked v-model="mode">1s
      <input name="mode" value="400" type="radio" v-model="mode">0.4s
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
        for(let i = 0; i < this.randomColor.length; i++) {
            await this.timeout(Number(this.mode))
            this.currentColor = this.randomColor[i]
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
            console.log(1)
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