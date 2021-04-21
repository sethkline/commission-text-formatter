<template>
  <div class="container">
    <nav class="navbar is-fixed-top">
      <a
        class="button is-primary is-pulled-left"
        href="https://contract.ritterim.com/staff/"
        target="_blank"
        >Link to Contracting app</a
      >
    </nav>
    <section class="section">
      <h1 class="is-size-3">Commission Formatter</h1>
      <label class="label pt-3">
        Paste product title
        <input class="input" v-model="title" type="text" />
      </label>
      <br />
      <label class="label">
        Paste product commissions
        <textarea class="textarea" v-model="text"></textarea>
      </label>
      <div class="pt-3">
        <h2 class="is-size-4">Results</h2>
        <quill-editor :content="formatText" :options="editorOption" ref="quill">
        </quill-editor>
      </div>
    </section>
  </div>
</template>

<script>
import Vue from "vue";
import VueQuillEditor from "vue-quill-editor";

import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";

Vue.use(VueQuillEditor);

export default {
  name: "editor",
  data() {
    return {
      content: "",
      text: "",
      title: "",
      editorOption: {
        // some quill options
        modules: {
          toolbar: [["bold"]],
        },
      },
    };
  },
  computed: {
    formatTitle() {
      if (this.title.includes(":")) {
        return this.title.replace(":", "");
      }
      return this.title;
    },
    formatText() {
      if (this.title && this.text) {
        const sections = this.text.split("|");
        const lines = sections.map((line) => {
          const split = line.split(":");
          return `${split[1].trim()} - ${split[0].trim()}`;
        });
        const allLines = lines.join("</p><p>");
        return `
          <p><strong>${this.formatTitle}</strong></p>
          <p>${allLines}</p>
        `;
      }
      return "";
    },
  },
};
</script>
