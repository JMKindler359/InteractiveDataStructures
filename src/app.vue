<template>
  <div>
    <h1>Bogosort Visualizer</h1>
    <p class="description">
      Bogosort randomly shuffles the array until it is sorted.
    </p>
    <div class="array">
      <div
        v-for="(height, index) in array"
        :key="index"
        class="bar"
        :style="{ height: height + 'px' }"
      ></div>
    </div>
    <button @click="startBogosort" :disabled="isSorting">Start Sorting</button>
    <button @click="reshuffleArray" :disabled="isSorting">Reshuffle</button>
  </div>
</template>

<script>
export default {
data() {
  return {
    array: [50, 200, 30, 150, 100], // Sample array of heights
    isSorting: false,
  };
},
methods: {
  // Shuffle the array randomly
  shuffleArray() {
    for (let i = this.array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [this.array[i], this.array[j]] = [this.array[j], this.array[i]]; // Swap
    }
  },

  // Check if the array is sorted
  isSorted() {
    for (let i = 0; i < this.array.length - 1; i++) {
      if (this.array[i] > this.array[i + 1]) {
        return false; // Array is not sorted
      }
    }
    return true; // Array is sorted
  },

  // Main Bogosort function
  async startBogosort() {
    this.isSorting = true; // Disable button while sorting

    // Keep shuffling until the array is sorted
    while (!this.isSorted()) {
      this.shuffleArray(); // Shuffle the array

      // We need a small delay to visualize the sorting process
      await new Promise((resolve) => {
        setTimeout(() => {
          resolve();
        }, 100); // Adjust delay time for a slower or faster visualization
      });
    }

    this.isSorting = false; // Enable button once sorting is done
  },

  // Reshuffle the array manually
  reshuffleArray() {
    this.shuffleArray();
  }
},
};
</script>

<style scoped>
.array {
display: flex;
justify-content: center;
gap: 5px;
align-items: flex-end;
height: 300px;
}

.bar {
width: 30px;
background-color: #4CAF50;
transition: height 0.2s ease-in-out;
}

button {
margin: 10px;
padding: 10px 15px;
font-size: 16px;
cursor: pointer;
border: none;
background-color: #008CBA;
color: white;
border-radius: 5px;
}

button:hover {
background-color: #005f73;
}

button:disabled {
background-color: gray;
cursor: not-allowed;
}
</style>
