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

      <br>
      <button @click="onClickS3PresignUrl()">Presigned URL</button>
      <div v-if="presignedUrl">
        {{ presignedUrl }}
      </div>

      <br>
      <button @click="onClickS3Put()">Upload Image</button>

      <amplify-sign-out />
    </amplify-authenticator>
  </div>
</template>

<script>
import { API, Storage } from 'aws-amplify'

export default {
  data () {
    return {
      content: '',
      urlShortening: '',
      presignedUrl: ''
    }
  },
  computed: {
    level () {
      return 'protected'
    },
    objectKey () {
      return 'test/test.jpg'
    },
    expires () {
      return 120
    },
    filePath () {
      return require('@/assets/img/test.jpg')
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
          original: this.$config.originalUrl
        }
      }
      const { shortening }  = await API.post('URLShortening', '/url-shortening', init)
      this.urlShortening = shortening
    },
    async onClickS3PresignUrl () {
      this.presignedUrl = await Storage.get(this.objectKey, { expires: this.expires, level: this.level })
    },
    async onClickS3Put () {
      const result = await Storage.put(this.objectKey, this.filePath, { level: this.level })
      console.log(result)
    }
  }
}
</script>