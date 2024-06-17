<template>
  <v-main
    id="scrollContainer"
    ref="scrollContainer"
    v-scroll.self="onScroll"
    style="overflow-y: auto; height: 100vh"
    class="hide-scrollbar"
  >
    <v-container fluid>
      <v-row
        v-for="(message, message_index) in messages"
        :key="'message_box_' + message_index"
        :justify="message.senderId === currentUserId ? 'end' : 'start'"
      >
        <v-col cols="auto" :order="message.senderId === currentUserId ? 5 : 0">
          <v-avatar :image="id_user_map[message.senderId].avatar" />
        </v-col>
        <v-col cols="auto" style="max-width: 45%">
          <v-hover v-slot="{ isHovering, props }">
            <v-fab
              :active="isHovering"
              v-bind="props"
              icon="mdi-dots-vertical"
              absolute
              offset
              variant="flat"
              density="compact"
              @click="actionMenu"
            />
            <v-card v-bind="props">
              <v-card-text> {{ message.text }}</v-card-text>
            </v-card>
          </v-hover>
        </v-col>
      </v-row>

      <v-menu v-model="action_menu_state" :target="action_menu_target">
        <v-list :density="actionMenuStyle.density">
          <v-list-item
            v-for="(action, action_index) in actions"
            :key="'action_' + action_index"
            :title="action.title"
            @click="actionHandler(action)"
          >
            <template #prepend>
              <v-icon :icon="action.icon" size="small" />
            </template>
          </v-list-item>
        </v-list>
      </v-menu>
      <v-fab
        :active="scroll_buttom"
        icon="mdi-arrow-down-bold"
        location="top end"
        app
        offset
        @click="scrollBottom"
        :style="{ left: '50%', transform: 'translateX(-50%)' }"
      />
    </v-container>
  </v-main>
</template>

<script>
export default {
  props: {
    currentUserId: {
      type: String,
    },
    users: {
      type: Array,
      default: () => [],
    },
    messages: {
      type: Array,
      default: () => [],
    },
    actions: {
      type: Array,
      default: () => [],
    },
    actionMenuStyle: {
      type: Object,
      default: () => {},
    },
  },

  data() {
    return {
      action_menu_state: false,
      action_menu_target: null,
      scroll_buttom: false,
      id_user_map: {},
    };
  },

  created() {
    for (const user of this.users) {
      this.id_user_map[user.id] = user;
    }
  },

  methods: {
    actionMenu($event) {
      this.action_menu_state = true;
      this.action_menu_target = [$event.clientX, $event.clientY];
    },
    actionHandler(action) {
      this.$emit("select:action", action);
    },
    onScroll($event) {
      const scrollTop = $event.target.scrollTop;
      const scrollHeight = $event.target.scrollHeight;
      const clientHeight = $event.target.clientHeight;
      if (scrollTop === 0) {
        this.$emit("scrollTop");
      } else if (scrollHeight - scrollTop > clientHeight) {
        this.scroll_buttom = true;
      } else {
        this.scroll_buttom = false;
      }
    },
    scrollBottom() {
      // TODO
      this.$refs.scrollContainer.$el.scrollIntoView(true);
    },
  },
};
</script>

<style scoped>
.hide-scrollbar {
  -ms-overflow-style: none; /* IE 和 Edge */
  scrollbar-width: none; /* Firefox */
}

.hide-scrollbar::-webkit-scrollbar {
  display: none; /* Chrome, Safari 和 Opera */
}
</style>
