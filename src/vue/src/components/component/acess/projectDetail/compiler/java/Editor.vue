<template>
  <div class="editor-container">
    <div class="code-container">
      <Codemirror v-model:value="modifyCode"
                :options="cmOptions"
                style="width: 100%"/>
    </div>
    <button class="run-btn"
            type="button"
            @click="runCompile">run</button>
  </div>
</template>

<script>
import Codemirror from "codemirror-editor-vue3";
import "codemirror-editor-vue3/dist/style.css"
import "codemirror/theme/dracula.css"
import "codemirror/mode/javascript/javascript.js";
import {mapActions, mapState} from "vuex";

export default {
  name: "Editor",
  computed: {
    ...mapState({
      code: state => state.javaCompile.code
    })
  },
  components: { Codemirror },
  data() {
    return {
      cmOptions: {
        mode: 'text/javascript',
        theme: 'dracula',
        lineNumbers: true,
        smartIndent: true,
        indentUnit: 2,
        foldGutter: true,
        styleActiveLine: true,
      },
      modifyCode: this.code
    }
  },
  methods: {
    ...mapActions({
      updateCode: 'javaCompile/updateCode',
      runCompile: 'javaCompile/runCompile'
    }),
  },
  watch: {
    "$store.state.javaCompile.code"(e) {
      this.modifyCode = e
    },
    modifyCode(e) {
      this.updateCode(e)
    }
  }
}
</script>

<style scoped>
.editor-container {
  position: relative;
}

.open-file {
  height: 40px;
  display: flex;
}

.file {
  border: 1px solid #999;
  width: 160px;
  background: #2C2F3B;
  font-size: 16px;
  color: #999;
  display: flex;
  align-items: center;
  padding: 0 10px;
  cursor: pointer;
}

.file button {
  color: #999;
}

.file button:hover {
  color: #fff;
}

.file img {
  margin-right: 10px;
}

.file-name {
  margin-right: auto;
}

.clicked-file {
  color: #fff;
  border-color: #fff;
}

.more-file {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: auto;
}

.code-container {
  height: 100%;
}

.code-container > * {
  font-size: 20px;
}

.run-btn {
  position: absolute;
  top: 15px;
  right: 10px;
  z-index: 10;
}

.run-btn:hover {
  color: #00ff00;
}
</style>