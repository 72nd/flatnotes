<template>
  <div ref="editorElement"></div>
</template>

<script setup>
import Editor from "@toast-ui/editor";
import { onMounted, ref } from "vue";

import baseOptions from "./baseOptions.js";

const props = defineProps({
  initialValue: String,
  initialEditType: {
    type: String,
    default: "markdown",
  },
  addImageBlobHook: Function,
  attachFileHandler: Function,
});

const emit = defineEmits(["change", "keydown"]);

const editorElement = ref();
let toastEditor;

onMounted(() => {
  toastEditor = new Editor({
    ...baseOptions,
    el: editorElement.value,
    initialValue: props.initialValue,
    initialEditType: props.initialEditType,
    events: {
      change: () => {
        emit("change");
      },
      keydown: (_, event) => {
        emit("keydown", event);
      },
    },
    hooks: props.addImageBlobHook
      ? { addImageBlobHook: props.addImageBlobHook }
      : {},
  });
  toastEditor.insertToolbarItem(
    { groupIndex: 4, itemIndex: 0 },
    {
      el: createAttachFileButton(),
      name: "attachFile",
      tooltip: "Attach File",
    },
  );
});

function createAttachFileButton() {
  const button = document.createElement("button");
  button.className = "toastui-editor-toolbar-icons";
  button.style.backgroundImage = "none";
  button.style.margin = "0";
  button.textContent = "📎";
  button.addEventListener("click", openFileDialog);
  return button;
}

function openFileDialog() {
  // Create file input dynamically
  const fileInput = document.createElement("input");
  fileInput.type = "file";
  fileInput.style.display = "none";
  
  // Handle file selection
  fileInput.addEventListener("change", (event) => {
    const file = event.target.files[0];
    if (file && props.attachFileHandler) {
      props.attachFileHandler(file, insertLink);
    }
    // Clean up: remove the file input after use
    fileInput.remove();
  });
  
  // Trigger file selection dialog
  fileInput.click();
}

function insertLink(url, filename) {
  // Use replaceSelection to insert raw markdown without escaping
  const markdown = `[${filename}](${url})`;
  const editor = toastEditor.getCurrentModeEditor();
  editor.replaceSelection(markdown);
}

function getMarkdown() {
  return toastEditor.getMarkdown();
}

function setMarkdown(markdown) {
  toastEditor.setMarkdown(markdown);
}

function isWysiwygMode() {
  return toastEditor.isWysiwygMode();
}

defineExpose({ getMarkdown, setMarkdown, isWysiwygMode });
</script>

<style>
@import "@toast-ui/editor/dist/toastui-editor.css";
@import "prismjs/themes/prism.css";
@import "@toast-ui/editor-plugin-code-syntax-highlight/dist/toastui-editor-plugin-code-syntax-highlight.css";
@import "./toastui-editor-overrides.scss";
</style>
