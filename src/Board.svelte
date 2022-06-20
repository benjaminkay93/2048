<script>
  const KEY_UP = 'ArrowUp'
  const KEY_DOWN = 'ArrowDown'
  const KEY_LEFT = 'ArrowLeft'
  const KEY_RIGHT = 'ArrowRight'

  let defaultBoard = {
    0: 0,
    1: 0,
    2: 0,
    3: 0,
    4: 0,
    5: 0,
    6: 0,
    7: 0,
    8: 0,
    9: 0,
    10: 0,
    11: 0,
    12: 0,
    13: 0,
    14: 0,
    15: 0,
  }

  const COLUMNS = [[0,4,8,12],[1,5,9,13],[2,6,10,14],[3,7,11,15]]
  const ROWS = [[0,1,2,3],[4,5,6,7],[8,9,10,11],[12,13,14,15]]

  export let board = {
    ...defaultBoard
  }

  let emptyCells = [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]

  const geneateNewPosition = (min, max) => { // min and max included 
    return Math.floor(Math.random() * (max - min + 1) + min)
  }

  const countEmpty = () => {
    return Object.values(board).reduce((acc, cell, index)=> {
      if(cell) return acc;
      return acc + 1
    }, 0)
  }

  const resetEmptyCells = () => {
    let newEmptyCells = []
    Object.keys(board).forEach(value => {
      if (board[value] === 0) {
        newEmptyCells.push(value)
      }
    })
    emptyCells = [...newEmptyCells]
  }

  const resetBoard = () => {
    board = {...defaultBoard}
    resetEmptyCells()
    console.log('reset', board, emptyCells)
  }

  const boardCount = 0;

  const placeNewNumber = () => {
    if (emptyCells.length !== 0) {
      let indexToFill = geneateNewPosition(0, emptyCells.length - 1)

      if (indexToFill > -1) {
        board[emptyCells[indexToFill]] = Math.random() <= 0.75 ? 2 : 4
        emptyCells.splice(indexToFill, 1)
      }
    }
  }

  const sortArray = array => {
    var start = 0;
    for (let index = 1; index < array.length; index++) {
      const cell = array[index];
      if (cell) {
        const target = array[start];
        if (!target || target === cell ) {
          array[start] += cell ;
          array[index] = 0;
        } else if (start + 1 < index) {
          index--;
        }
        if (target) { start++; }
      }
    }
    return array;
}

  const shiftUp = () => {
    COLUMNS.forEach(column => {
      const array = column.map(cell => board[cell])
      const sortedArray = sortArray(array)
      column.forEach((cell, index) => {
        board[cell] = sortedArray[index]
      })
    })
  }

  const shiftDown = () => {
    COLUMNS.forEach(column => {
      const reversedColumn = [...column].reverse()
      const array = reversedColumn.map(cell => board[cell])
      const sortedArray = sortArray(array)
      reversedColumn.forEach((cell, index) => {
        board[cell] = sortedArray[index]
      })
    })
  }
  const shiftLeft = () => {
    ROWS.forEach(row => {
      const array = row.map(cell => board[cell])
      const sortedArray = sortArray(array)
      row.forEach((cell, index) => {
        board[cell] = sortedArray[index]
      })
    })
  }
  const shiftRight = () => {
    ROWS.forEach(row => {
      const reversedRow = [...row].reverse()
      const array = reversedRow.map(cell => board[cell])
      const sortedArray = sortArray(array)
      reversedRow.forEach((cell, index) => {
        board[cell] = sortedArray[index]
      })
    })
  }

  const SHIFT_STRATEGY = {
    [KEY_UP]: shiftUp,
    [KEY_DOWN]: shiftDown,
    [KEY_LEFT]: shiftLeft,
    [KEY_RIGHT]: shiftRight,
  }

  document.addEventListener('keydown', (event) => {
    var name = event.key;
    var code = event.code;

    SHIFT_STRATEGY[event.key]()
    resetEmptyCells()

    placeNewNumber()

    console.log({name, code, countEmpty: countEmpty()})
  })
</script>
  
<section>
  {#each Object.values(board) as cell, x}
    {#if !cell}
      <div class="cell {cell}">
      </div>
    {:else}
      <div class="cell number-{cell}">
        {cell}
      </div>
    {/if}
  {/each}
</section>
<button on:click={resetBoard}>Reset Board</button>

<style>
  section {
    margin-left: auto;
    margin-right: auto;
    width: calc(4.5rem * 4);
    height: calc(4.5rem * 4);
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(4, 1fr);
    grid-column-gap: 0.5rem;
    grid-row-gap: 0.5rem;
    background-color: rgb(3, 9, 78);
    padding: 1rem;
    border-radius: 1rem;
  }

  .cell {
    width: 100%;
    height: 100%;
    background-color: rgb(196, 198, 212);
    color: rgb(31, 35, 76);
    text-transform: uppercase;
    font-size: 2em;
    font-weight: 800;
    border-radius: 0.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .number-2 {
    background-color: #ede4da;
    color: #766e65;
  }
  .number-4 {
    background-color: #EDDFC8;
    color: #766e65;
  }
  .number-8 {
    background-color: #f2b178;
    color: #f9f6f1;
  }
  .number-16 {
    background-color: #f59662;
    color: #f9f6f1;
  }
  .number-32 {
    background-color: #f67d5f;
    color: #f9f6f1;
  }
  .number-64 {
    background-color: #f65e3a;
    color: #f9f6f1;
  }

  .number-128 {
    background-color: #edcf73;
    color: #f9f6f1;
  }
  .number-256 {
    background-color: #eecc61;
    color: #f9f6f1;
  }
  .number-512 {
    background-color: #edc850;
    color: #f9f6f1;
  }
  .number-1024 {
    background-color: #eec63e;
    color: #f9f6f1;
  }
  .number-2048 {
    background-color: #edc22d;
    color: #f9f6f1;
  }


  
    
</style>

<!--
  2
  4
  8
  16
  32
  64
  128
  256
  512
  1024
  2048
  4096
  8192
  16384
  32768
  65536
-->