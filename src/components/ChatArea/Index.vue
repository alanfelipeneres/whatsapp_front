<template>
  <div class="chat-area">
    <div class="image full-width full-height">
      <Message
        v-for="item in messages"
        :key="item.id"
        :text="item.text"
        :hour="item.hour"
        :my="my(item.userId)"
      />
    </div>
  </div>
</template>

<script>
import Message from "src/components/Message/Index";
import api from "src/services/api";
import { notify } from "src/utils";

export default {
  name: "ChatArea",
  props: ["currentUser", "newMessages"],
  data() {
    return {
      messages: [],
      myId: localStorage.getItem("myId"),
    };
  },
  components: {
    Message,
  },
  async mounted() {
    console.log("", "getMessages");
    this.getMessages();
  },
  watch: {
    async currentUser() {
      this.messages = [];
      await this.getMessages();
    },
    async newMessages() {
      await this.getMessages();
    },
  },
  methods: {
    async getMessages() {
      await api
        .get(`messages/${this.currentUser}/${this.myId}`)
        .then((response) => {
          this.messages = response.data.sort((a, b) => a.id - b.id).reverse();
        })
        .catch((error) => {
          notify("negative", error.response.data.message);
        });
    },

    my(messageUserId) {
      return this.myId === messageUserId.toString();
    },
  },
};
</script>

<style scoped lang="scss">
.chat-area {
  height: calc(100vh - 59px - 62px);
  width: calc(100vw - 410px);
  background-color: $grey-background;
}

.image {
  background-image: url("../../assets/background.png");
  overflow: auto;
  display: flex;
  flex-direction: column-reverse;
}
</style>
