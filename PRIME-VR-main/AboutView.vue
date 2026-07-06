<template>
  <div class="container">
    <div class="squares-container">
      <!-- Square elements -->
      <div class="squares" ref="squares">
        <div
          v-for="(square, index) in squares"
          :key="index"
          class="square-container"
          :style="{
            transform: `translateX(${squarePositions[index]}px)`
          }"
        >
          <div class="square">
            <div class="top-round-square"></div>
            <div class="inner-square"></div>
            <span>{{ 'Text ' + (index + 1) }}</span>
          </div>

          <!-- Scroll Image placed beside each square -->
          <div class="scroll-image" @click="moveSquaresRight(index)">
            <img src="./next.png" alt="Scroll Image" />
          </div>
        </div>
      </div>
    </div>

    <div class="about">
      <h1>Our Company</h1>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      squares: new Array(6),
      currentTranslateX: 0,  
      squareWidth: 300,
      gap: 80,
      squarePositions: [0, 380, 760, 1140, 1520, 1900], 
    };
  },
  methods: {
    moveSquaresRight(index) {
      // Calculate the next positions based on the squareWidth + gap
      const maxTranslateX = -((this.squares.length - 1) * (this.squareWidth + this.gap));

      // Move all squares to the left by (squareWidth + gap) when the image is clicked
      this.squarePositions = this.squarePositions.map((pos, i) => {
        if (pos <= maxTranslateX) {
          // Reset position of the square that goes off-screen (to the right)
          return this.squarePositions[this.squares.length - 1] + this.squareWidth + this.gap;
        } else {
          // Otherwise, just move the square left by (squareWidth + gap)
          return pos - (this.squareWidth + this.gap);
        }
      });
    },
  },
};
</script>

<style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    text-align: center;
  }

  .squares-container {
    display: flex;
    flex-direction: row;
    align-items: center;
    position: relative;
    width: 80%;
    justify-content: flex-start;
  }

  .squares {
    display: flex;
    transition: transform 0.5s ease;
    gap: 80px;
  }

  .square-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    transition: transform 0.5s ease;
  }

  .square {
    background-color: #FF5448;
    width: 300px;
    height: 400px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;
  }

  .top-round-square {
    background-color: #fcada5;
    width: 200px;
    height: 50px;
    border-radius: 25px;
    position: absolute;
    top: -35px;
  }

  .inner-square {
    background-color: #ff9b90;
    width: 220px;
    height: 50%;
    border-radius: 15px;
    margin-bottom: 70px;
  }

  .square span {
    color: white;
    font-size: 18px;
    font-weight: 500;
    margin-top: 5px;
  }


  .scroll-image {
    cursor: pointer;
    z-index: 10;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    left: 100%; 
    margin-left: 20px; 
  }

  .scroll-image img {
    width: 40px;
    height: 40px;
  }

  .about {
    margin-top: 50px;
  }

  .about h1 {
    font-size: 36px;
    font-weight: 600;
    text-align: center;
  }
</style>
