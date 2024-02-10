<template>
  <div class="screen">
    <h1>Flip Card Memory</h1>
    <p>Select mode to start game</p>
    <div class="modes">
      <!-- truyền vào số lượng thẻ cần render để biết chế độ nào được chọn -->
      <button @click="onStart(16)">
        <span>4x4</span>
        <span>Normal</span>
      </button>
      <button @click="onStart(36)">
        <span>6x6</span>
        <span>Hard</span>
      </button>
      <button @click="onStart(64)">
        <span>8x8</span>
        <span>Hard</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    // Emit event "onStart" ra component cha kèm theo dữ liệu là số lượng thẻ truyền vào
    onStart(quantity) {
      this.$emit("onStart", { quantity });
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: var(--dark);
  color: var(--light);
}

.screen h1 {
  font-size: 4.5rem;
  text-transform: uppercase;
  @media (max-width: 46.1875em) {
    font-size: 3.5rem;
  }
}

.screen p {
  font-size: 2rem;
  @media (max-width: 46.1875em) {
    font-size: 2rem;
  }
}

.modes {
  display: flex;
  margin-top: 2rem;
}

.modes button {
  font: var(--font);
  width: 150px;
  height: 150px;
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
  @media (max-width: 46.1875em) {
    width: 100px;
    height: 100px;
    margin: 0 0.5rem;
  }
}

.modes button:hover {
  background-color: var(--light);
  color: var(--dark);
}

.modes button span:first-child {
  font-size: 2rem;
}

.modes button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
</style>
