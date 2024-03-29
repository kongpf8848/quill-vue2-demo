<template>
  <div class="my-editor">
    <quill-editor
      style="width: 100%"
      ref="myQuillEditor"
      :options="editorOption"
      @blur="onEditorBlur($event)"
      @focus="onEditorFocus($event)"
      @ready="onEditorReady($event)"
      @change="onEditorChange($event)"
    />

    <div style="width: 100%">
      <textarea
        style="width: 100%"
        ref="textareaRef"
        placeholder="请粘贴纪要json字符串"
        v-model="content"
        rows="30"
      />
    </div>
  </div>
</template>

<script>
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";
import Quill from "quill";
import Delta from "quill-delta";
import { quillEditor } from "vue-quill-editor";

export default {
  name: "RichEditor",
  components: {
    quillEditor,
  },
  props: {
    msg: String,
  },
  data() {
    return {
      content: "",
      editorOption: {
        placeholder: "请输入内容",
        modules: {
          toolbar: {
            container: [
              ["bold", "italic", "underline", "strike"],
              ["blockquote", "code-block"],
              [{ header: 1 }, { header: 2 }],
              [{ list: "ordered" }, { list: "bullet" }],
              [{ script: "sub" }, { script: "super" }],
              [{ indent: "-1" }, { indent: "+1" }],
              [{ direction: "rtl" }],
              [{ size: ["small", false, "large", "huge"] }],
              [{ header: [1, 2, 3, 4, 5, 6, false] }],
              [{ color: [] }, { background: [] }],
              [{ font: [] }],
              [{ align: [] }],
              ["link"],
              ["clean"],
              ["undo"],
              ["redo"],
            ],
            handlers: {
              undo: () => {
                this.editor.history.undo();
              },
              redo: () => {
                this.editor.history.redo();
              },
              output: () => {
                console.log(
                  "+++++++++quill-json:" +
                    JSON.stringify(this.editor.editor.delta.ops)
                );
              },
            },
          },
        },
      },
    };
  },
  methods: {
    onEditorBlur(quill) {
      console.log("onEditorBlur:", quill);
    },
    onEditorFocus(quill) {
      console.log("onEditorFocus:", quill);
    },
    onEditorReady(quill) {
      console.log("onEditorReady:", quill);
    },
    onEditorChange({ quill, html, text }) {
      console.log("onEditorChange:", text);
      var json = JSON.stringify(this.editor.editor.delta.ops);
      if (json !== this.content) {
        this.content = json;
      }
    },
    formatJson() {
      this.content = JSON.stringify(this.content, null, 4);
    },
  },
  computed: {
    editor() {
      return this.$refs.myQuillEditor.quill;
    },
  },
  mounted() {
    var undo = document.querySelector(".ql-undo");
    undo.innerHTML = "undo";
    var redo = document.querySelector(".ql-redo");
    redo.innerHTML = "redo";
  },
  watch: {
    content: {
      handler(newVal, oldVal) {
        if (newVal) {
          var json = JSON.stringify(this.editor.editor.delta.ops);
          if (newVal !== json) {
            console.log("+++++++++++++update editor from content");
            try {
              let delta = JSON.parse(newVal);
              this.editor?.setContents(delta);
            } catch (e) {
              console.log("parse json error:", e);
            }
          }
        } else {
          this.editor?.setContents([]);
        }
      },
      immediate: false,
    },
  },
};
</script>

<style>
.my-editor {
  width: 100%;
  height: 100%;
  min-height: 340px;
  min-width: 600px;
  display: flex;
  flex-direction: row;
  gap: 10px;
}
</style>
