<template>
    <div>
        <div class="gameStatus" :class="gameStatusColor">
            {{gameStatusMessage}}

        </div>
        <div>
            <table class="grid">
                <tr>
                    <cell name="1"></cell>
                    <cell name="2"></cell>
                    <cell name="3"></cell>
                </tr>
                <tr>
                    <cell name="4"></cell>
                    <cell name="5"></cell>
                    <cell name="6"></cell>
                </tr>
                <tr>
                    <cell name="7"></cell>
                    <cell name="8"></cell>
                    <cell name="9"></cell>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
import Cell from './Cell';
export default {
    components: { Cell },
    data() {
        return {
            // can be X or O
            activePlayer: 'O',
            // maintain the status of the game - win, draw or turn
            gameStatus: 'turn',
            gameStatusMessage: `O's turn`,
            // status color is used as the background color in the status bar
            // it can hold the name of the either of the following classes
            // statusTurn (default) is yellow for a turn
            // statusWin is green for a win
            // statusDraw is purple for a draw
            gameStatusColor: 'statusTurn',
            // no of moves played by both players in a sinlge game (max=9)
            moves: 0,
            // store the placement of X and O in the cells y their cell number
            cells: {
                1: '', 2: '', 3: '',
                4: '', 5: '', 6: '',
                7: '', 8: '', 9: '',
            },
            // contains all 8 possible winning conditions 
            windConditions: [
                [1, 2, 3], [4, 5, 6], [7, 8, 9], // rows
                [1, 4, 7], [2, 5, 8], [3, 6, 9], // columns
                [1, 5, 9], [3, 5, 7]             // diagonals
            ],

        }
    },
    computed: {
        // helper function to get the non active player
        nonActivePlayer() {
            if (this.activePlayer === 'O') {
                return 'X'
            }
            return 'O'
        }
    },
    watch: {
        // watches for the changes in the game status and changes the status message and color accordingly
        activePlayer() {
            if (this.gameStatus === 'win') {
                this.gameStatusColor = 'statusWin'
                this.gameStatusMessage = `${this.activePlayer} wins!`
                return
            }
            else if (this.gameStatus === 'draw') {
                this.gameStatusColor = 'statusDraw'
                this.gameStatusMessage = 'Draw!'
                return
            }
            this.gameStatusMessage = `${this.activePlayer}'s turn`
        }
    },
    methods: {
        changePlayer() {
            this.activePlayer = this.nonActivePlayer
        },

        // returns the game status to the gameStatus property
        changeGameStatus() {
            if (this.checkForWin()) {
                return this.gameIsWon()
            }
            // checks if the game is not yet won and all the 9 cells are filled
            else if (this.moves === 9) {
                // sets the status to draw
                return 'draw'
            }
            
            // sets the status to  turn
            return 'turn'
        },
        // check for all possible win conditions
        checkForWin() {
            for (let i = 0; i < this.windConditions.length; i++) {
                let wc = this.windConditions[i]
                let cells = this.cells
                // compare 3 cell values based on conditions
                if (this.areEqual(cells[wc[0]], cells[wc[1]], cells[wc[2]])) {
                    return true
                }
            }
            return false
        },
        areEqual() {
            var len = arguments.length
            for (let i = 1; i < len; i++) {
                if (arguments[i] === '' || arguments[i] != arguments[i - 1]) {
                    return false
                }
            }
            return true
        },
        gameIsWon() {
            // fires win event to change the score
            this.$parent.$emit('win', this.activePlayer)

            // sets the game status message
            this.gameStatusMessage = `${this.activePlayer} wins!`

            // fires an event for the Cell to freeze     
            this.$emit('freeze')
            return 'win'
        }
    },
    created() {
        // listen for a strike made by the user on cell
        // this is called by the cell component
        this.$on('strike', (cellNumber) => {
            // sets either X or O as the current player
            this.cells[cellNumber] = this.activePlayer
            // increment the number of moves
            this.moves++
            this.gameStatus = this.changeGameStatus()
            this.changePlayer()
        })
    },
}
</script>

<style>
.grid {
    background-color: #34495e;
    color: #fff;
    width: 100%;
    border-collapse: collapse;
}

.gameStatus {
    margin: 0px;
    padding: 15px;
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
    background-color: #f1c40f;
    color: #fff;
    font-size: 1.4em;
    font-weight: bold;
}

.statusTurn {
    background-color: #f1c40f;
}

.statusWin {
    background-color: #2ecc71;
}

.statusDraw {
    background-color: #9b59b6;
}
</style>