<template>
  <div class="granny-square-designer">
    <h2>Granny Square Designer</h2>

    <div class="design-controls">
      <ColorPalette
        ref="colorPalette"
        @colors-updated="handleColorsUpdated"
      />

      <div class="square-customizer">
        <h3>Square Design</h3>
        <div class="square-design-container">
          <div class="color-selectors" :key="`color-selectors-${paletteUpdateKey}`">
            <div class="color-select-section">
              <h4>Center:</h4>
              <div class="color-squares-container">
                <div
                  v-for="(color, index) in colors"
                  :key="`center-${color}-${index}`"
                  class="color-square"
                  :class="{ 'active': currentSquare.center === color }"
                  :style="{ backgroundColor: color }"
                  @click="setColor('center', color)"
                ></div>
              </div>
            </div>

            <div class="color-select-section">
              <h4>Petals:</h4>
              <div class="color-squares-container">
                <div
                  v-for="(color, index) in colors"
                  :key="`petal-${color}-${index}`"
                  class="color-square"
                  :class="{ 'active': currentSquare.petals === color }"
                  :style="{ backgroundColor: color }"
                  @click="setColor('petals', color)"
                ></div>
              </div>
            </div>

            <div class="color-select-section">
              <h4>Field:</h4>
              <div class="color-squares-container">
                <div
                  v-for="(color, index) in colors"
                  :key="`field-${color}-${index}`"
                  class="color-square"
                  :class="{ 'active': currentSquare.field === color }"
                  :style="{ backgroundColor: color }"
                  @click="setColor('field', color)"
                ></div>
              </div>
            </div>

            <div class="action-buttons">
              <button @click="addSquare" class="add-btn">Add Square to Blanket</button>
              <button @click="randomizeColors" class="random-btn">Randomize Colors</button>
            </div>
          </div>

          <div class="square-preview-column">
            <div class="square-preview-container">
              <h4>Preview</h4>
              <div class="granny-square preview-square" :style="{ backgroundColor: currentSquare.field }">
              <div class="petal-container">
                <div
                  v-for="circle in dashedCircles"
                  :key="circle.id"
                  class="dashed-circle"
                  :style="{
                    width: `${circle.size}%`,
                    height: `${circle.size}%`,
                    opacity: circle.opacity,
                    strokeDasharray: circle.dashArray
                  }"
                ></div>

                <div class="petal petal-top" :style="{ backgroundColor: currentSquare.petals }"></div>
                <div class="petal petal-top-right" :style="{ backgroundColor: currentSquare.petals }"></div>
                <div class="petal petal-right" :style="{ backgroundColor: currentSquare.petals }"></div>
                <div class="petal petal-bottom-right" :style="{ backgroundColor: currentSquare.petals }"></div>
                <div class="petal petal-bottom" :style="{ backgroundColor: currentSquare.petals }"></div>
                <div class="petal petal-bottom-left" :style="{ backgroundColor: currentSquare.petals }"></div>
                <div class="petal petal-left" :style="{ backgroundColor: currentSquare.petals }"></div>
                <div class="petal petal-top-left" :style="{ backgroundColor: currentSquare.petals }"></div>

                <div class="triangle-connector top" :style="{ borderTopColor: currentSquare.petals }"></div>
                <div class="triangle-connector top-right" :style="{ borderTopColor: currentSquare.petals }"></div>
                <div class="triangle-connector right" :style="{ borderTopColor: currentSquare.petals }"></div>
                <div class="triangle-connector bottom-right" :style="{ borderTopColor: currentSquare.petals }"></div>
                <div class="triangle-connector bottom" :style="{ borderTopColor: currentSquare.petals }"></div>
                <div class="triangle-connector bottom-left" :style="{ borderTopColor: currentSquare.petals }"></div>
                <div class="triangle-connector left" :style="{ borderTopColor: currentSquare.petals }"></div>
                <div class="triangle-connector top-left" :style="{ borderTopColor: currentSquare.petals }"></div>

                <div class="center" :style="{ backgroundColor: currentSquare.center }"></div>
              </div>
              </div>
              <div class="texture-toggle">
                <label class="toggle-container">
                  <input type="checkbox" v-model="showTexture">
                  <span class="toggle-label">Texture Mode</span>
                </label>
                <div class="texture-warning">May slow page at high square counts</div>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>

    <div class="blanket-container">
      <h3>Your Granny Square Blanket</h3>
      <div class="blanket" :style="{ gridTemplateColumns: `repeat(${gridWidth}, 1fr)` }">
        <div
          v-for="(square, index) in squares"
          :key="index"
          class="granny-square blanket-square"
          :style="{ backgroundColor: square.field }"
        >
          <div class="petal-container">
            <div
              v-for="circle in dashedCircles"
              :key="circle.id"
              class="dashed-circle"
              :style="{
                width: `${circle.size}%`,
                height: `${circle.size}%`,
                opacity: circle.opacity,
                strokeDasharray: circle.dashArray
              }"
            ></div>

            <div class="petal petal-top" :style="{ backgroundColor: square.petals }"></div>
            <div class="petal petal-top-right" :style="{ backgroundColor: square.petals }"></div>
            <div class="petal petal-right" :style="{ backgroundColor: square.petals }"></div>
            <div class="petal petal-bottom-right" :style="{ backgroundColor: square.petals }"></div>
            <div class="petal petal-bottom" :style="{ backgroundColor: square.petals }"></div>
            <div class="petal petal-bottom-left" :style="{ backgroundColor: square.petals }"></div>
            <div class="petal petal-left" :style="{ backgroundColor: square.petals }"></div>
            <div class="petal petal-top-left" :style="{ backgroundColor: square.petals }"></div>

            <div class="triangle-connector top" :style="{ borderTopColor: square.petals }"></div>
            <div class="triangle-connector top-right" :style="{ borderTopColor: square.petals }"></div>
            <div class="triangle-connector right" :style="{ borderTopColor: square.petals }"></div>
            <div class="triangle-connector bottom-right" :style="{ borderTopColor: square.petals }"></div>
            <div class="triangle-connector bottom" :style="{ borderTopColor: square.petals }"></div>
            <div class="triangle-connector bottom-left" :style="{ borderTopColor: square.petals }"></div>
            <div class="triangle-connector left" :style="{ borderTopColor: square.petals }"></div>
            <div class="triangle-connector top-left" :style="{ borderTopColor: square.petals }"></div>

            <div class="center" :style="{ backgroundColor: square.center }"></div>
          </div>
          <button @click="removeSquare(index)" class="remove-btn">×</button>
        </div>
      </div>

      <div class="random-squares-generator">
        <div class="random-squares-input">
          <input
            type="number"
            min="1"
            max="100"
            v-model.number="randomSquaresCount"
            class="random-count-input"
          >
          <button @click="addRandomSquares" class="add-random-btn">
            Add {{ randomSquaresCount }} Random Square{{ randomSquaresCount !== 1 ? 's' : '' }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ColorPalette from './ColorPalette.vue'

export default {
  name: 'GrannySquareDesigner',
  components: {
    ColorPalette
  },
  data () {
    return {
      gridWidth: 8,
      currentSquare: {
        center: '#8ed2f0',
        petals: '#c2ed7b',
        field: '#d89629'
      },
      squares: [],
      defaultColors: [
        '#8ed2f0',
        '#c2ed7b',
        '#d8b029',
        '#5b1621',
        '#f2aecf',
        '#dd8536',
        '#d2dde7',
        '#4a5b2d'
      ],
      paletteColors: [],
      paletteUpdateKey: 0,
      randomSquaresCount: 1,
      showTexture: true
    }
  },
  computed: {
    colors () {
      return this.paletteColors.length > 0 ? this.paletteColors : this.defaultColors
    },
    dashedCircles () {
      if (!this.showTexture) return []
      
      const circlesConfig = []
      const totalCircles = 45

      for (let i = 1; i <= totalCircles; i++) {
        const t = i / totalCircles
        const size = 160 - (Math.pow(t, 0.6) * 160)
        const opacity = 0.4
        const dashWidth = 1 + (i % 1)
        const gapWidth = 1 + ((i + 1) % 1)
        const dashArray = `${dashWidth},${gapWidth}`

        circlesConfig.push({
          id: `circle-${i}`,
          size,
          opacity,
          dashArray
        })
      }

      return circlesConfig
    }
  },
  methods: {
    getRandomColor (excludeColors = []) {
      const availableColors = this.colors.filter(color => !excludeColors.includes(color))

      if (availableColors.length === 0) {
        return excludeColors.length > 0 ? '#000000' : this.colors[0]
      }

      return availableColors[Math.floor(Math.random() * availableColors.length)]
    },
    randomizeColors () {
      if (this.colors.length >= 3) {
        const centerColor = this.getRandomColor()
        const petalColor = this.getRandomColor([centerColor])
        const fieldColor = this.getRandomColor([centerColor, petalColor])

        this.currentSquare = {
          center: centerColor,
          petals: petalColor,
          field: fieldColor
        }
      } else if (this.colors.length > 0) {
        this.currentSquare = {
          center: this.getRandomColor(),
          petals: this.getRandomColor(),
          field: this.getRandomColor()
        }
      }
    },
    addSquare () {
      this.squares.push({...this.currentSquare})
      this.randomizeColors()
    },
    removeSquare (index) {
      this.squares.splice(index, 1)
    },
    setColor (part, color) {
      this.currentSquare[part] = color
    },
    handleColorsUpdated (updatedColors) {
      this.paletteColors = [...updatedColors]
      this.paletteUpdateKey++
    },
    addRandomSquares () {
      const count = Math.max(1, parseInt(this.randomSquaresCount) || 1)

      for (let i = 0; i < count; i++) {
        const centerColor = this.getRandomColor()
        const petalColor = this.getRandomColor([centerColor])
        const fieldColor = this.getRandomColor([centerColor, petalColor])

        this.squares.push({
          center: centerColor,
          petals: petalColor,
          field: fieldColor
        })
      }

      this.randomizeColors()
    }
  },
  watch: {
    colors: {
      handler (newColors) {
        if (newColors.length > 0) {
          if (!newColors.includes(this.currentSquare.center)) {
            this.currentSquare.center = newColors[0]
          }
          if (!newColors.includes(this.currentSquare.petals)) {
            this.currentSquare.petals = newColors[0]
          }
          if (!newColors.includes(this.currentSquare.field)) {
            this.currentSquare.field = newColors[0]
          }
        }
      },
      deep: true
    }
  },
  mounted () {
    this.$nextTick(() => {
      if (this.$refs.colorPalette) {
        this.paletteColors = [...this.$refs.colorPalette.colors]
      }
      this.randomizeColors()
    })
  }
}
</script>

<style>
.granny-square-designer {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.granny-square-designer h2,
.granny-square-designer h3,
.granny-square-designer h4 {
  margin-top: 0;
}

.design-controls {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  background-color: #f5f5f5;
  padding: 1.5rem;
  border-radius: 8px;
  border: 1px solid #ddd;
}

.square-customizer {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 1rem;
  background-color: white;
}

.square-design-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  min-height: 300px;
}

.color-selectors {
  flex: 1;
  margin-right: 1rem;
}

.square-preview-column {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  min-width: 50%;
}

.square-preview-container {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.granny-square {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1px;
  border-radius: 3px;
  position: relative;
  width: 120px;
  overflow: hidden;
}

.preview-square {
  width: 120px;
  height: 120px;
  margin: 0.5rem 0;
}

.petal-container {
  position: relative;
  width: 100%;
  height: 100%;
}

.dashed-circle {
  position: absolute;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  pointer-events: none;
  z-index: 10;
  background: transparent;
  box-sizing: border-box;
  border: 1px dashed rgba(255, 255, 255, .9);
  mix-blend-mode: overlay;
}

.triangle-connector {
  position: absolute;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 40px 13px 0;
  border-color: transparent;
  z-index: 1;
}

.triangle-connector.top {
  top: 17px;
  left: 47px;
}

.triangle-connector.top-right {
  top: 21px;
  right: 28px;
  transform: rotate(45deg);
}

.triangle-connector.right {
  top: 40px;
  left: 70px;
  transform: rotate(90deg);
}

.triangle-connector.bottom-right {
  bottom: 21px;
  right: 28px;
  transform: rotate(135deg);
}

.triangle-connector.bottom {
  bottom: 17px;
  left: 47px;
  transform: rotate(180deg);
}

.triangle-connector.bottom-left {
  bottom: 21px;
  left: 28px;
  transform: rotate(-135deg);
}

.triangle-connector.left {
  top: 40px;
  right: 70px;
  transform: rotate(-90deg);
}

.triangle-connector.top-left {
  top: 21px;
  left: 28px;
  transform: rotate(-45deg);
}

.petal {
  position: absolute;
  width: 26px;
  height: 26px;
  border-radius: 50%;
  z-index: 3;
}

.petal-top {
  top: 3px;
  left: 47px;
}

.petal-top-right {
  top: 11px;
  right: 11px;
}

.petal-right {
  top: 47px;
  right: 3px;
}

.petal-bottom-right {
  bottom: 11px;
  right: 11px;
}

.petal-bottom {
  bottom: 3px;
  left: 47px;
}

.petal-bottom-left {
  bottom: 11px;
  left: 11px;
}

.petal-left {
  top: 47px;
  left: 3px;
}

.petal-top-left {
  top: 11px;
  left: 11px;
}

.center {
  position: absolute;
  width: 24%;
  height: 24%;
  top: 38%;
  left: 38%;
  border-radius: 50%;
  z-index: 4;
}

.color-selectors {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 1rem;
}

.color-select-section {
  margin-bottom: 0.5rem;
}

.color-select-section h4 {
  margin: 0 0 0.5rem 0;
  font-size: 1rem;
}

.color-squares-container {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.color-square {
  width: 30px;
  height: 30px;
  border-radius: 4px;
  cursor: pointer;
  border: 1px solid #ddd;
}

.color-square:hover {
  transform: scale(1.1);
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
  transition: all 0.2s;
}

.color-square.active {
  border: 3px solid #333;
  padding: 1px;
}

.help-text {
  color: #666;
  font-size: 0.9rem;
}

.add-btn, .random-btn {
  margin-right: 0.5rem;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  border: none;
}

.add-btn {
  background-color: #4CAF50;
  color: white;
}

.add-btn:hover {
  background-color: #45a049;
}

.random-btn {
  background-color: #3498db;
  color: white;
}

.random-btn:hover {
  background-color: #2980b9;
}

.blanket-container {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 1.5rem;
  background-color: #f9f9f9;
}

.blanket {
  display: grid;
  gap: 1px;
  background-color: #333;
  padding: 2px;
  border-radius: 4px;
  width: 983px;
}

.blanket-square {
  aspect-ratio: 1;
}

.remove-btn {
  position: absolute;
  top: 0;
  right: 0;
  width: 24px;
  height: 24px;
  border-radius: 0 0 0 8px;
  background-color: rgba(255, 85, 85, 0.8);
  color: white;
  border: none;
  font-size: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.2s;
  z-index: 30;
}

.granny-square:hover .remove-btn {
  opacity: 1;
}

.random-squares-generator {
  margin-top: 1.5rem;
  display: flex;
  justify-content: center;
}

.random-squares-input {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.random-count-input {
  width: 60px;
  text-align: center;
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.add-random-btn {
  background-color: #9C27B0;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.2s;
}

.add-random-btn:hover {
  background-color: #7B1FA2;
}

.texture-toggle {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  text-align: center;
  margin-bottom: 0.5rem;
}

.toggle-container {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.toggle-container input[type="checkbox"] {
  margin-right: 0.5rem;
}

.toggle-label {
  font-size: 0.9rem;
  font-weight: bold;
}

.texture-warning {
  font-size: 0.75rem;
  color: #888;
  margin-top: 0.25rem;
}

@media (max-width: 768px) {
  .granny-square-designer {
    gap: 1rem;
  }

  .random-squares-input {
    flex-direction: column;
    gap: 0.5rem;
  }
}
</style>