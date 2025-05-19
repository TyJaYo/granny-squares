<template>
  <div class="granny-square">
    <div class="square-preview">
      <div class="field" :style="{ backgroundColor: fieldColor }">
        <div class="petals" :style="{ backgroundColor: petalColor }">
          <div class="center" :style="{ backgroundColor: centerColor }"></div>
        </div>
      </div>
    </div>
    <div class="color-selectors">
      <div class="color-select">
        <label>Center:</label>
        <select v-model="centerColor">
          <option v-for="(color, index) in availableColors" :key="`center-${index}`" :value="color">
            {{ color }}
          </option>
        </select>
        <div class="color-preview" :style="{ backgroundColor: centerColor }"></div>
      </div>
      <div class="color-select">
        <label>Petals:</label>
        <select v-model="petalColor">
          <option v-for="(color, index) in availableColors" :key="`petal-${index}`" :value="color">
            {{ color }}
          </option>
        </select>
        <div class="color-preview" :style="{ backgroundColor: petalColor }"></div>
      </div>
      <div class="color-select">
        <label>Field:</label>
        <select v-model="fieldColor">
          <option v-for="(color, index) in availableColors" :key="`field-${index}`" :value="color">
            {{ color }}
          </option>
        </select>
        <div class="color-preview" :style="{ backgroundColor: fieldColor }"></div>
      </div>
    </div>
    <button @click="$emit('remove')" class="remove-btn">Remove Square</button>
  </div>
</template>

<script>
export default {
  name: 'GrannySquare',
  props: {
    availableColors: {
      type: Array,
      required: true
    },
    initialColors: {
      type: Object,
      default: () => ({
        center: '#FF5733',
        petals: '#33FF57',
        field: '#3357FF'
      })
    }
  },
  data () {
    return {
      centerColor: this.initialColors.center,
      petalColor: this.initialColors.petals,
      fieldColor: this.initialColors.field
    }
  },
  watch: {
    availableColors: {
      handler (newColors) {
        if (newColors.length > 0) {
          if (!newColors.includes(this.centerColor)) {
            this.centerColor = newColors[0]
          }
          if (!newColors.includes(this.petalColor)) {
            this.petalColor = newColors[0]
          }
          if (!newColors.includes(this.fieldColor)) {
            this.fieldColor = newColors[0]
          }
        }
      },
      deep: true
    }
  }
}
</script>

<style>
.granny-square {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 1rem;
  margin-bottom: 1rem;
  background-color: #f9f9f9;
}

.square-preview {
  width: 120px;
  height: 120px;
  margin: 0 auto 1rem auto;
}

.field {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.petals {
  width: 70%;
  height: 70%;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
}

.center {
  width: 40%;
  height: 40%;
  border-radius: 50%;
}

.color-selectors {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.color-select {
  display: flex;
  align-items: center;
}

.color-select label {
  width: 70px;
  text-align: right;
  margin-right: 0.5rem;
}

.color-select select {
  flex-grow: 1;
  padding: 0.25rem;
}

.color-select .color-preview {
  width: 20px;
  height: 20px;
  margin-left: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.remove-btn {
  background-color: #ff5555;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
}

.remove-btn:hover {
  background-color: #ff3333;
}
</style>