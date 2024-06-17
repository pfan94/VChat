<template>
  <v-app-bar>
    <template #prepend>
      <v-list-item
        :prepend-avatar="avatar"
        :title="title"
        :subtitle="subtitle"
      />
    </template>
    <template #append>
      <v-menu>
        <template #activator="{ props }">
          <v-btn icon="mdi-dots-vertical" v-bind="props" />
        </template>
        <v-list>
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
    </template>
  </v-app-bar>
</template>
<script>
export default {
  props: {
    avatar: {
      type: String,
      default: () => "",
    },
    title: {
      type: String,
      default: () => "",
    },
    subtitle: {
      type: String,
      default: () => "",
    },
    actions: {
      type: Array,
      default: () => [],
    },
  },

  methods: {
    actionHandler(action) {
      this.$emit("select:action", action);
    },
  },
};
</script>
