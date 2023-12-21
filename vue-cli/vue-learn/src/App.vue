<template>
  <div id="app">
    <h1>Text Replacer</h1>
    <div class="container">
      <div class="input">
        <h2>Input</h2>
        <textarea v-model="input" placeholder="Enter your text here"></textarea>
      </div>
      <div class="output">
        <h2>Output</h2>
        <textarea v-model="output" readonly></textarea>
      </div>
    </div>
    <div class="options">
      <label for="find">Find:</label>
      <input id="find" v-model="find" placeholder="Enter the text to find">
      <label for="replace">Replace with:</label>
      <input id="replace" v-model="replace" placeholder="Enter the text to replace with">
    </div>
  </div>
</template>

<script>
import { ref, computed, watch } from 'vue'

export default {
  setup() {
    // Define the reactive data
    const input = ref('') // The input text
    const output = ref('') // The output text
    const find = ref('') // The text to find
    const replace = ref('') // The text to replace with

    // Define the computed property that returns the replaced text
    const replaced = computed(() => {
      // If the find text is empty, return the input text
      if (!find.value) return input.value
      // Otherwise, use a regular expression to replace all occurrences of the find text with the replace text
      // and wrap the replaced text with <mark> tags to highlight it
      const regex = new RegExp(find.value, 'g')
      return input.value.replace(regex, `<mark>${replace.value}</mark>`)
    })

    // Define the watcher that updates the output text when the replaced text changes
    watch(replaced, (newValue) => {
      // Set the output text to the replaced text
      output.value = newValue
      // Get the output textarea element
      const outputElement = document.querySelector('.output textarea')
      // Set the inner HTML of the output textarea element to the replaced text
      // This allows the <mark> tags to be rendered as HTML
      outputElement.innerHTML = newValue
    })

    // Return the data and the computed property to the template
    return {
      input,
      output,
      find,
      replace,
      replaced
    }
  }
}
</script>

<style>
  /* Define some basic styles for the web page */
  #app {
    width: 800px;
    margin: 0 auto;
  }

  .container {
    display: flex;
    justify-content: space-between;
  }

  .input, .output {
    width: 48%;
  }

  textarea {
    width: 100%;
    height: 300px;
    font-family: monospace;
    font-size: 16px;
    padding: 10px;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }

  .options {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
  }

  label {
    font-weight: bold;
  }

  input {
    width: 200px;
    padding: 5px;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }

  mark {
    background-color: yellow;
  }
</style>
