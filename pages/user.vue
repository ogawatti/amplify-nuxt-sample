<template>
  <div class="user">
    <amplify-authenticator>
      <button @click="onClickHelloButton()">Hello</button>
      <div v-if="content">
        {{ content }}
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
      content: ''
    }
  },
  methods: {
    async onClickHelloButton () {
      this.content = await API.get('hello', '/world')
    }
  }
}
</script>