<template>
  <section class="container">
    <div>
      <div>
        <div v-html="PrismicDOM.RichText.asText(content.welcomemessage, linkResolver)" />

      </div>
      <nuxt-link to="product">TEST</nuxt-link>

      <pre>{{ content.welcomemessage }}</pre>

      <div class="links">

        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green">Documentation</a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey">GitHub</a>
      </div>
    </div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
var Prismic = require('prismic-javascript');
var PrismicDOM = require('prismic-dom');
var apiEndpoint = "https://peyotedesign.prismic.io/api/v2";

export default {
  async asyncData() {
    const api = await Prismic.getApi(apiEndpoint);
    const data = await api.query(
          Prismic.Predicates.at('document.type', 'homepage'),
    );
    //attach the match to the content
    const content = data.results[0].data;
    return { content };
  },
  components: {
    AppLogo
  },
  data() {
    return {
      PrismicDOM,
      linkResolver (doc) {
        console.log(doc)
        if (doc.type === 'product') {
            return '/product/' + doc.uid;
        }
        return '/';
      }
    }
  },
  methods: {

  }
}
</script>

<style>

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
