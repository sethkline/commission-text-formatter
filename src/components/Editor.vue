<template>
  <div class="container">
    <nav class="navbar is-fixed-top">
      <a class="button is-link is-pulled-left" href="https://contract.ritterim.com/staff/" target="_blank"
        >Link to Contracting app</a
      >
      <button @click="reset" class="button is-primary">Reset</button>
    </nav>
    <section class="section">
      <h1 class="is-size-3">Commission Formatter</h1>
      <br />
      <label class="label">
        Paste product commissions from contracting app
        <textarea class="textarea" v-model="text"></textarea>
      </label>
      <button class="button" @click="parseText">Format</button>
      <div class="pt-3">
        <h2 class="is-size-4">Results</h2>
        <quill-editor :content="formatText" :options="editorOption" ref="quill"> </quill-editor>
      </div>
    </section>
  </div>
</template>

<script>
import Vue from 'vue';
import VueQuillEditor from 'vue-quill-editor';

import 'quill/dist/quill.core.css';
import 'quill/dist/quill.snow.css';

Vue.use(VueQuillEditor);

export default {
  name: 'editor',
  data() {
    return {
      content: '',
      text: '',
      title: '',
      products: [],
      formatText: '',
      editorOption: {
        // some quill options
        modules: {
          toolbar: [['bold']]
        }
      }
    };
  },
  methods: {
    reset() {
      this.content = '';
      this.text = '';
      this.formatText = '';
    },
    formatLines(text) {
      const sections = text.split('|');
      const lines = sections.map((line) => {
        const split = line.split(':');
        if (split && split[1] && split[0]) {
          return `${split[1].trim()} - ${split[0].trim()}`;
        }
        return '';
      });
      return lines.join('</p><p>');
    },
    parseText() {
      const products = this.text.split(/\n/);
      const finalProducts = products.map((product) => {
        let [title, ...commissions] = product.split(':');
        commissions = commissions.join(':');
        const formatCommission = this.formatLines(commissions);
        return `
          <p><strong>${title}</strong></p>
          <p>${formatCommission}</p>
        `;
      });
      this.formatText = finalProducts.join('');
    }
  }
};
</script>
