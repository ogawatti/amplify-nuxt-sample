<template>
  <div class="user">
    <amplify-authenticator>
      <button @click="onClickHelloButton()">Hello</button>
      <div v-if="content">
        {{ content }}
      </div>

      <br>
      <button @click="onClickUrlShorteningButton()">URL Shortening</button>
      <div v-if="urlShortening">
        {{ urlShortening }}
      </div>
      <amplify-sign-out />
    </amplify-authenticator>
  </div>
</template>

<script>
import { API } from 'aws-amplify'

export default {
  data () {
    return {
      content: '',
      urlShortening: ''
    }
  },
  methods: {
    async onClickHelloButton () {
      this.content = await API.get('hello', '/world')
    },
    async onClickUrlShorteningButton () {
      console.log('URL Shortening')
      const init = {
        body: {
          original: 'https://ogawatti.net/video/?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=xxxxxxxxxx&X-Amz-Date=xxxxxxxxxx&X-Amz-Expires=xxxxxxxxxx&X-Amz-SignedHeaders=host&X-Amz-Signature=xxxxxxxxxx'
        }
      }
      const { shortening }  = await API.post('URLShortening', '/url-shortening', init)
      this.urlShortening = shortening
    }
  }
}
</script>