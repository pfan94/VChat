<template>
  <v-layout height="100%">
    <!-- sidebar -->
    <chat-rooms
      :currentRoom="room"
      :rooms="rooms"
      :actions="roomActions"
      :roomsStyle="roomsStyle"
      :actionsStyle="roomActionsStyle"
      @select:room="onRoomSelect"
      @add:room="onAddRoom"
      @select:action="onRoomAction"
    />
    <!-- appbar -->
    <chat-window-bar
      :actions="barActions"
      :avatar="chatWindowAvatar"
      :title="chatWindowTitle"
      :subtitle="chatWindowSubtitle"
      @select:action="onBarAction"
    />
    <!-- main -->
    <chat-window
      :users="users"
      :currentUserId="currentUserId"
      :messages="messages"
      :actions="messageActions"
      :actionMenuStyle="messageActionMenuStyle"
      @select:action="onMessageAction"
      @load:messages="onLoadMessages"
    />
    <!-- footer -->
    <chat-input-bar @send:message="onMessageSend" />
  </v-layout>
</template>

<script>
import ChatRooms from "./components/ChatRooms.vue";
import ChatWindow from "./components/ChatWindow.vue";
import ChatWindowBar from "./components/ChatWindowBar.vue";
import ChatInputBar from "./components/ChatInputBar.vue";

export default {
  props: {
    // Chat Rooms props
    rooms: {
      type: Array,
      default: () => [],
    },
    roomActions: {
      type: Array,
      default: () => [{ title: "delete", icon: "mdi-delete" }],
    },
    roomsStyle: {
      type: Object,
      default: () => ({ density: "comfortable" }),
    },
    roomActionsStyle: {
      type: Object,
      default: () => ({ density: "compact" }),
    },
    // chat window props
    users: {
      type: Array,
      default: () => [],
    },
    currentUserId: {
      type: String,
    },
    messages: {
      type: Array,
      default: () => [],
    },
    messageActions: {
      type: Array,
      default: () => [
        { title: "forward", icon: "mdi-share" },
        { title: "delete", icon: "mdi-delete" },
      ],
    },
    messageActionMenuStyle: {
      type: Object,
      default: () => ({ density: "compact" }),
    },
    // chat window bar props
    barActions: {
      type: Array,
      default: () => [{ title: "delete", icon: "mdi-delete" }],
    },
    chatWindowAvatar: {
      type: String,
      default: () => "",
    },
    chatWindowTitle: {
      type: String,
      default: () => "",
    },
    chatWindowSubtitle: {
      type: String,
      default: () => "",
    },
  },

  components: {
    ChatRooms,
    ChatWindow,
    ChatWindowBar,
    ChatInputBar,
  },

  data() {
    return {
      room: {},
      input_text: null,
    };
  },

  methods: {
    onRoomSelect(room) {
      this.room = room;
      this.$emit("select:room", room);
    },
    onRoomAction(action) {
      this.$emit("select:room-action", action);
    },
    onAddRoom() {
      this.$emit("add:room");
    },
    onMessageAction(action) {
      this.$emit("select:message-action", action);
    },
    onBarAction(action) {
      this.$emit("select:bar-action", action);
    },
    onMessageSend(text) {
      this.$emit("send:message", text);
    },
    onLoadMessages() {
      this.$emit("load:messages");
    },
  },
};
</script>
