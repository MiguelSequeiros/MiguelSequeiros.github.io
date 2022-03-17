<template>
  <div id="app">
    <textarea
      id="input"
      v-model="ddl"
    ></textarea>
		<textarea
      readonly
      ref="output"
      id="output"
      @focus="$event.target.select()"
      :value="json"
    ></textarea>
    <button @click="copyToClipboard">Copy</button>
		<button @click="convert">Convert</button>
  </div>
</template>

<script>
import { Parser } from 'sql-ddl-to-json-schema'

export default {
  name: 'App',
  data() {
    return {
      ddl: '',
      json: '',
      parser: new Parser('mariadb')
    }
  },
  methods: {
    convert () {
      this.parser.feed(this.ddl)
      const b = this.parser.results
      const compactJson = this.parser.toCompactJson(b)
      this.json = JSON.stringify(this.parser.toJsonSchemaArray({useRef: false}, compactJson))
      console.log(this.json)
    },
    copyToClipboard () {
      // copy this json to clipboard
      this.$refs.output.focus();
      document.execCommand('copy');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#input {
  width: 30%;
  height: 300px;
  font-size: 14px;
  line-height: 18px;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
}
#output {
  width: 30%;
  height: 300px;
  font-size: 14px;
  line-height: 18px;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
}
</style>
