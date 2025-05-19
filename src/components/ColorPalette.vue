<template>
  <div class="color-palette">
    <h3>Color Palette</h3>
    <div class="color-list">
      <div
        v-for="(color, index) in colors"
        :key="index"
        class="color-item"
      >
        <input
          type="text"
          class="color-value-input"
          v-model="colors[index]"
          @change="validateColor(index)"
        >
        <input
          type="color"
          class="color-picker"
          v-model="colors[index]"
        >
        <button @click="removeColor(index)" class="color-remove-btn">×</button>
      </div>
      <button @click="addNewColor" class="add-color-btn">
        <span>+</span> Add Color
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ColorPalette',
  data () {
    return {
      colors: [
        '#8ed2f0',
        '#c2ed7b',
        '#d8b029',
        '#5b1621',
        '#f2aecf',
        '#dd8536',
        '#d2dde7',
        '#4a5b2d'
      ]
    }
  },
  methods: {
    addNewColor () {
      const randomColor = '#' + Math.floor(Math.random()*16777215).toString(16).padStart(6, '0')
      this.colors.push(randomColor)
      this.$emit('colors-updated', this.colors)
    },
    removeColor (index) {
      this.colors.splice(index, 1)
      this.$emit('colors-updated', this.colors)
    },
    getRandomColor () {
      if (this.colors.length === 0) return '#000000'
      return this.colors[Math.floor(Math.random() * this.colors.length)]
    },
    validateColor (index) {
      const hexColorRegex = /^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/
      if (!hexColorRegex.test(this.colors[index])) {
        this.colors[index] = '#8ED2F0'
      }
      this.$emit('colors-updated', this.colors)
    }
  }
}
</script>

<style>
.color-palette {
  margin-bottom: 2rem;
  border: 1px solid #ddd;
  padding: 1rem;
  border-radius: 5px;
}

.add-color-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f5f5f5;
  border: 2px dashed #ccc;
  border-radius: 4px;
  padding: 0.5rem;
  cursor: pointer;
  width: 200px;
  height: 39px;
  font-size: 0.9rem;
  color: #555;
  transition: all 0.2s;
}

.add-color-btn:hover {
  background-color: #e9e9e9;
  border-color: #aaa;
}

.add-color-btn span {
  font-size: 1.2rem;
  margin-right: 0.5rem;
}

.color-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px 60px;
}

.color-item {
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  padding: 0.5rem;
  border-radius: 4px;
  position: relative;
  width: 200px;
}

.color-preview {
  width: 20px;
  height: 20px;
  border-radius: 4px;
  margin-right: 0.5rem;
  border: 1px solid #ddd;
}

.color-value-input {
  width: 80px;
  font-family: monospace;
  padding: 0.25rem;
  border: 1px solid #ddd;
  border-radius: 3px;
  font-size: 0.9rem;
  margin-right: 0.5rem;
}

.color-picker {
  width: 90px;
  height: 25px;
  padding: 0;
  border: 1px solid #ddd;
  margin-right: 0.5rem;
  cursor: pointer;
}

.color-remove-btn {
  cursor: pointer;
  background: none;
  border: none;
  font-size: 1.2rem;
  color: slategray;
  position: absolute;
  left: -10px;
  top: 48%;
  transform: translateY(-50%);
}
</style>