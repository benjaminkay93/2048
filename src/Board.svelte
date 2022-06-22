<script lang="ts">
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

  const COLUMNS: number[][] = [[0,4,8,12],[1,5,9,13],[2,6,10,14],[3,7,11,15]]
  const ROWS: number[][] = [[0,1,2,3],[4,5,6,7],[8,9,10,11],[12,13,14,15]]

  export let board = {
    ...defaultBoard
  }

  let emptyCells = [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]

  const geneateNewPosition = (min:number, max: number): number => { // min and max included 
    return Math.floor(Math.random() * (max - min + 1) + min)
  }

  const countEmpty = () => {
    return Object.values(board).reduce((acc, cell)=> {
      if(cell) return acc;
      return acc + 1
    }, 0)
  }

  const resetEmptyCells = () => {
    let newEmptyCells: number[] = []
    Object.keys(board).forEach(value => {
      if (board[value] === 0) {
        newEmptyCells.push(parseInt(value, 10))
      }
    })
    emptyCells = [...newEmptyCells]
  }

  const resetBoard = () => {
    board = {...defaultBoard}
    resetEmptyCells()
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

  const play = (direction) => {
    const oldBoard = {...board}

    SHIFT_STRATEGY[direction]()
    resetEmptyCells()

    console.log(JSON.stringify(oldBoard) !== JSON.stringify(board))

    if(JSON.stringify(oldBoard) !== JSON.stringify(board) || emptyCells.length === 16) {
      placeNewNumber()
    }
  }

  let xDown = null;                                                        
  let yDown = null;                                          
                                                                          
  const handleTouchStart = (event) => {
      const firstTouch = event.touches[0];                                      
      xDown = firstTouch.clientX;                                      
      yDown = firstTouch.clientY;                                      
  };                                                
                                                                          
  const handleTouchMove = (event) => {
    if ( ! xDown || ! yDown ) {
      return;
    }
    var xUp = event.touches[0].clientX;
    var yUp = event.touches[0].clientY;
    var xDiff = xDown - xUp;
    var yDiff = yDown - yUp;
    if ( Math.abs(xDiff) < 1 || Math.abs(yDiff) < 1) {
      return;
    }

    xDown = null;
    yDown = null;

    if ( Math.abs(xDiff) > Math.abs(yDiff) ) {
        if ( xDiff > 0 ) {
          play(KEY_LEFT)
          return
        }
        play(KEY_RIGHT)
        return;
    }
    if ( yDiff > 0 ) {
      play(KEY_UP)
      return;
    }
    play(KEY_DOWN)
  };

  document.addEventListener('touchstart', handleTouchStart);        
  document.addEventListener('touchmove', handleTouchMove);
  document.addEventListener('keydown', (event) => play(event.key));
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
<button class="reset-button" on:click={resetBoard}>Reset Board</button>

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
    background-color: var(--mg);
    padding: 1rem;
    border-radius: 1rem;
  }

  .cell {
    width: 100%;
    height: 100%;
    background-color: var(--cell);
    color: var(--bg);
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

  .reset-button {
    margin-top: 16px;
    height: 36px;
    width: 200px;
    border: 1px solid var(--cell);
    border-radius: 18px;
    padding: 8px;
    background-color: var(--mg);
    color: var(--cell);
    font-weight: 700;
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