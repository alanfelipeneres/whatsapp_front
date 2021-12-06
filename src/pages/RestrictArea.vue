<template>
  <q-page class="container full-height">
    <div>
      <ConversationArea :users="users" @selectedItem="setCurrentItem" />
    </div>
    <div class="column" v-if="selectedItem">
      <TopBar :title="nameConversation" />
      <ChatArea :currentUser="selectedItem" :newMessages="newMessages" />
      <MessageBar :currentUser="selectedItem" @reload="reloadMessages" />
    </div>
    <Empty v-else></Empty>
  </q-page>
</template>

<script>
import Empty from "src/components/Empty/Index";
import ConversationArea from "src/components/ConversationArea/Index";
import TopBar from "src/components/TopBar/Index";
import ChatArea from "src/components/ChatArea/Index";
import MessageBar from "src/components/MessageBar/Index";
import api from "src/services/api";
import { notify } from "src/utils";
import { socket } from "src/services/socket";

export default {
  name: "MainLayout",
  data() {
    return {
      users: [],
      newMessages: "",
      selectedItem: null,
      nameConversation: "",
      myId: localStorage.getItem("myId"),
    };
  },
  components: {
    Empty,
    ConversationArea,
    TopBar,
    ChatArea,
    MessageBar,
  },
  created() {
    const receiver = localStorage.getItem("receiver");
    //console.log("receiver", localStorage.getItem("receiver"));
    console.log("myid", localStorage.getItem("myid"));

    socket.on(localStorage.getItem("myid"), (message) => {
      console.log("teste", message);
      const arr = [];
      this.users.forEach((item) => {
        if (item.id === message.user_id) {
          item.newMessage = true;
        }

        arr.push(item);
      });

      this.newMessages = message.id;
      this.users = arr;
    });
  },

  async mounted() {
    await api
      .get("users")
      .then((response) => {
        const allUsers = [];
        for (const item of response.data) {
          if (item.id.toString() !== this.myId) {
            allUsers.push(item);
          }
        }

        this.users = allUsers;
      })
      .catch(() => {
        notify("negative", "Falha ao listar usuários!");
      });
  },

  methods: {
    async setCurrentItem({ id, email }) {
      console.log("clicou!");
      this.selectedItem = id;
      await api
        .get(`user/${email}`)
        .then((response) => {
          this.nameConversation = response.data.name;
        })
        .catch(() => {
          this.nameConversation = "Novo Usuário";
        });
    },
    reloadMessages({ messageId }) {
      this.newMessages = messageId;
    },
  },
};
</script>

<style scoped lang="scss">
.container {
  display: flex;
  overflow: hidden;
  min-width: 1200px;
}
</style>
