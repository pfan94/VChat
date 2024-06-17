<template>
  <v-footer app>
    <v-textarea
      v-model="input_text"
      variant="outlined"
      rows="1"
      hide-details
      placeholder="Type message"
      :loading="sending"
    />
    <v-btn icon="mdi-emoticon-outline" />
    <v-file-input style="display: none" />
    <v-btn icon="mdi-paperclip" />
    <v-btn icon="mdi-send" @click="sendMessage" />
  </v-footer>
</template>

<script>
export default {
  data() {
    return {
      input_text: null,
      sending: false,
    };
  },

  methods: {
    sendMessage() {
      console.log(this.input_text);
      this.$emit("send:message", this.input_text);
      this.sending = true;
      this.input_text = null;
    },
  },
};
</script>
