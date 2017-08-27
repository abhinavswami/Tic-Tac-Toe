<template>
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
</template>

<script>
export default {
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
    }
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