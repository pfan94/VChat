<template>
  <v-navigation-drawer>
    <template #prepend>
      <v-card-text>
        <v-text-field
          v-model="search_text"
          density="compact"
          variant="outlined"
          placeholder="search"
          hide-details
          prepend-inner-icon="mdi-magnify"
          @update:model-value="searchRoom"
        >
          <template #append>
            <v-tooltip text="Tooltip">
              <template v-slot:activator="{ props }">
                <v-btn
                  icon="mdi-plus"
                  @click="onAddRoom"
                  size="small"
                  v-bind="props"
                />
              </template>
            </v-tooltip>
          </template>
        </v-text-field>
      </v-card-text>
      <v-divider />
    </template>

    <v-list :density="roomsStyle.density" nav>
      <template
        v-for="(room, room_index) in searched_rooms"
        :key="'room_' + room_index"
      >
        <v-list-item
          :title="room.title"
          :subtitle="room.subtitle"
          :prepend-avatar="room.avatar"
          :active="room === currentRoom"
          @click="onRoomSelect(room)"
        >
          <template #append>
            <v-btn
              icon="mdi-dots-vertical"
              variant="text"
              size="x-small"
              @click="openActionMenu"
            />
          </template>
        </v-list-item>
        <v-divider />
      </template>
    </v-list>
    <v-menu v-model="action_menu_state" :target="action_menu_target">
      <v-list :density="actionsStyle.density">
        <v-list-item
          v-for="(action, action_index) in actions"
          :key="'action_' + action_index"
          :title="action.title"
          @click="onActionSelect(action)"
        >
          <template #prepend>
            <v-icon :icon="action.icon" size="small" />
          </template>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-navigation-drawer>
</template>
<script>
export default {
  props: {
    currentRoom: {
      type: Object,
      default: () => {},
    },
    rooms: {
      type: Array,
      default: () => [],
    },
    roomSearchFields: {
      type: Array,
      default: () => [],
    },
    actions: {
      type: Array,
      default: () => [],
    },
    roomsStyle: {
      type: Object,
      default: () => {},
    },
    actionsStyle: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      action_menu_state: false,
      action_menu_target: null,
      search_text: null,
      searched_rooms: [],
    };
  },
  created() {
    this.searched_rooms = this.rooms;
  },
  methods: {
    onRoomSelect(room) {
      this.$emit("select:room", room);
    },
    openActionMenu($event) {
      this.action_menu_state = true;
      this.action_menu_target = $event.target;
    },
    onActionSelect(action) {
      this.$emit("select:action", action);
    },
    onAddRoom() {
      this.$emit("add:room");
    },
    searchRoom() {
      if (!this.search_text) {
        this.searched_rooms = this.rooms;
        return;
      }
      this.searched_rooms = this.rooms.filter((room) =>
        room.title.toLowerCase().includes(this.search_text.toLowerCase())
      );
    },
  },
};
</script>
