<template>
  <div>
    <h1>Lets play tic-tac-toe!</h1>
    <table>
      <tr>
        <td @click='turn(0, 0)'>{{board[0][0].val}}</td>
        <td @click='turn(0, 1)'>{{board[0][1].val}}</td>
        <td @click='turn(0, 2)'>{{board[0][2].val}}</td>
      </tr>
      <tr>
        <td @click='turn(1, 0)'>{{board[1][0].val}}</td>
        <td @click='turn(1, 1)'>{{board[1][1].val}}</td>
        <td @click='turn(1, 2)'>{{board[1][2].val}}</td>
      </tr>
      <tr>
        <td @click='turn(2, 0)'>{{board[2][0].val}}</td>
        <td @click='turn(2, 1)'>{{board[2][1].val}}</td>
        <td @click='turn(2, 2)'>{{board[2][2].val}}</td>
      </tr>
    </table>
    <h3 v-if='!endResult'>{{player}}'s turn</h3>
    <h3 v-else-if="endResult=='X'">Congratulations, X!</h3>
    <h3 v-else-if="endResult=='O'">Congratulations, O!</h3>
    <h3 v-else>Stalemate!</h3>
  </div>
</template>

<script>
export default {
  data() {
    return {
      player: 'O',
      // endResult: '',
      movesLeft: 9,
      board: {
        0: {
          0: {val:'', locked:false},
          1: {val:'', locked:false},
          2: {val:'', locked:false}
        },
        1: {
          0: {val:'', locked:false},
          1: {val:'', locked:false},
          2: {val:'', locked:false}
        },
        2: {
          0: {val:'', locked:false},
          1: {val:'', locked:false},
          2: {val:'', locked:false}
        }
      }
    }
  },
  methods: {
    turn(i, j) {
      if (this.board[i][j].locked) {
        return //can't change previous moves
      }
      if (this.endResult) {
        return //the game is finished!
      }
      this.board[i][j].val = this.player //make the player's move
      this.player === 'X' ? this.player = 'O' : this.player = 'X' //change player
      this.board[i][j].locked = true //lock
      this.movesLeft--
    },
    eq(i, j, player) {
      //checks whether board is equal to a player at a coordinate, used for conciseness
      return this.board[i][j].val === player
    }
  },
  computed: {
    endResult() {
      for (const t of ['X', 'O']) {
        for (const i in this.board) {
          if (this.eq(i, 0, t) && this.eq(i, 1, t) && this.eq(i, 2, t)) return t //rows
          if (this.eq(0, i, t) && this.eq(1, i, t) && this.eq(2, i, t)) return t //cols
        }
        if (this.eq(0, 0, t) && this.eq(1, 1, t) && this.eq(2, 2, t)) return t //first diagonal
        if (this.eq(0, 2, t) && this.eq(1, 1, t) && this.eq(2, 0, t)) return t //second diagonal
      }
      if (this.movesLeft == 0) {
        return 'stalemate'
      }
      return ''
    }
  }
}
</script>

<style scoped>
table {
  margin: 0 auto;
  border-collapse: collapse;
  border-style: hidden;
  table-layout: fixed;
  white-space: nowrap;
}

table td {
  border: 10px solid rgb(6, 104, 126);
  font-size: 4rem;
  table-layout: fixed;
  width: 7rem;
  height: 7rem;
}

</style>