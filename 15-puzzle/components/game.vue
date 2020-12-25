<template>
  <div>
    <div class="data-for-game">
      <p class="name-game">15</p>
      <div class="number-of-moves">
        <p>Ходов</p>
        <p>{{ countOfMoves }}</p>
      </div>
      <div class="time">
        <p>Время</p>
        <p v-if="startGame === false">00:00</p>
        <p v-else-if="startGame">{{ this.time }}</p>
      </div>
    </div>
    <div>
      <button class="btn-start-game" v-on:click="loadGame">Начать игру</button>
      <transition-group name="cell-group" tag="div" class="container-for-cells">
        <div
          class="cell"
          v-for="(cell, index) in cells"
          v-bind:key="cell"
          v-on:click="clickOnCell(index)"
        >
          {{ cell }}
        </div>
      </transition-group>
      <div class="gameWon" v-if="gameIsWon">
        Поздравляю, Вы выиграли!
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      countOfMoves: 0,
      cells: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, ""],
      arrayForCheck: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, ""],
      startGame: false,
      date: 0,
      time: 0,
      gameIsWon: false,
    };
  },
  mounted() {
    setInterval(() => {
      let nowTime = new Date().getTime();
      let deffTime = nowTime - this.date;
      let minutes = Math.floor((deffTime % (1000 * 60 * 60)) / (1000 * 60));
      let seconds = Math.floor((deffTime % (1000 * 60)) / 1000);
      this.time =
        String(minutes).padStart(2, "0") +
        ":" +
        String(seconds).padStart(2, "0");
    }, 1000);
  },
  methods: {
    makeRandomSort() {
      this.cells.sort(() => Math.random() - 0.5);
    },
    clickOnCell(index) {
      if (this.startGame) {
        let valueCell = this.cells[index];
        if (this.cells[index - 1] == 0) {
          this.cells[index] = "";
          this.cells[index - 1] = valueCell;
          this.countOfMoves++;
        } else if (this.cells[index - 4] == 0) {
          this.cells[index] = "";
          this.cells[index - 4] = valueCell;
          this.countOfMoves++;
        } else if (this.cells[index + 1] == 0) {
          this.cells[index] = "";
          this.cells[index + 1] = valueCell;
          this.countOfMoves++;
        } else if (this.cells[index + 4] == 0) {
          this.cells[index] = "";
          this.cells[index + 4] = valueCell;
          this.countOfMoves++;
        }
      }
    },

    addedTime() {
      this.date = new Date().getTime();
    },

    emptyCellPosition() {
      this.cells.indexOf("");
    },

    loadGame() {
      this.makeRandomSort();
      this.startGame = true;
      this.countOfMoves = 0;
      this.gameIsWon = false;
      this.addedTime();
      this.checkCells();
    },

    checkCells() {
      let result = 0;
      for (let index = 0; index < this.arrayForCheck.length; index++) {
        if (this.arrayForCheck[index] == this.cells[index]) result++;
      }
      if (result > 15) this.gameIsWon = true;
    },
  },
};
</script>

<style lang="scss" scoped>
.btn-start-game {
  margin-bottom: 20px;
  padding: 5px 10px;
  font-size: 20px;
  color: #f5edfa;
  background-color: #696573;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: none;
}

.data-for-game {
  margin-bottom: 2rem;
  display: flex;
  flex-direction: row;

  .name-game {
    font-size: 80px;
    font-weight: 700;
    line-height: 80px;
    margin-right: 80px;
    color: #696573;
  }

  .number-of-moves,
  .time {
    background-color: #a8a0b8;
    padding: 5px;
    border-radius: 10px;
    width: 100px;
    height: 70px;
    font-weight: 700;
    font-size: 20px;

    p {
      text-align: center;
      color: #e6def3;
    }
  }

  .number-of-moves {
    margin-right: 20px;
  }
}

.gameWon {
  position: absolute;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 300px;
  height: 200px;
  z-index: 9999;
  top: 300px;
  left: 200px;
  background-color: #a69eb7;
  border: 2px solid;
  color: #696573;
  font-size: 20px;
  border-radius: 20px;
}

.container-for-cells {
  position: relative;
  width: 430px;
  height: 430px;
  background-color: #a69eb7;
  border-radius: 10px;
  display: flex;
  flex-wrap: wrap;
  padding: 5px;

  .cell {
    box-sizing: border-box;
    width: 100px;
    height: 100px;
    border-radius: 10px;
    font-weight: 700;
    font-size: 50px;
    cursor: pointer;
    text-align: center;
    transition: all 0.3s;
    color: #696573;
    background-color: #d6cce4;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 2px solid #696573;
    margin: 2px;
  }
}

.cell-group-move {
  transition: all 0.6s;
}
</style>