<template>
  <VNavigationDrawer
    v-model="drawerStatus"
    :width="265"
    :style="cssProps"
    app
    class="sidebarColor"
    fixed
    clipped
  >
    <VList>
      <template v-for="menu in menus">
        <VListTile v-if="!menu.children" :key="menu.title" :to="menu.path" exact ripple>
          <VListTileAction>
            <VIcon>{{ menu.icon }}</VIcon>
          </VListTileAction>
          <VListTileTitle>{{ menu.title }}</VListTileTitle>
        </VListTile>

        <VListGroup
          v-if="menu.children"
          :key="menu.title"
          :prepend-icon="menu.icon"
          :value="true"
          no-action
        >
          <VListTile slot="activator">
            <VListTileTitle>{{ menu.title }}</VListTileTitle>
          </VListTile>

          <VListTile
            v-for="submenu in menu.children"
            :key="submenu.title"
            :to="submenu.path"
            ripple
            exact
          >
            <VListTileAction>
              <VIcon>{{ submenu.icon }}</VIcon>
            </VListTileAction>
            <VListTileContent>
              <VListTileTitle>{{ submenu.title }}</VListTileTitle>
            </VListTileContent>
          </VListTile>
        </VListGroup>
      </template>
    </VList>
  </VNavigationDrawer>
</template>

<script>
import { mapState } from 'vuex';

export default {
  name: 'Sidebar',
  data() {
    return {
      userProfile: {},
      menus: [
        {
          title: 'Dashboard',
          icon: 'dashboard',
          path: ''
        },
        {
          title: 'Post Categories',
          icon: 'settings',
          path: '/categories'
        },
        {
          title: 'Users',
          icon: 'account_box',
          path: '/users'
        },
        {
          title: 'Feedbacks',
          icon: 'comment',
          path: '/feedbacks'
        },
        {
          title: 'Issues',
          icon: 'note',
          path: '/issues'
        }
      ]
    };
  },
  computed: {
    ...mapState('core', ['appName']),
    cssProps() {
      return {
        '--secondary-color': this.$vuetify.theme.secondary
      };
    },
    drawerStatus: {
      get() {
        return this.$store.state.layout.navDrawerToggle;
      },
      set(newValue) {
        this.$store.state.layout.navDrawerToggle = newValue;
      }
    }
  },
  methods: {
    menuGroupShouldBeExpanded(menu) {
      if (menu.childrenUrlMatcher) {
        return this.$route.fullPath.includes(menu.childrenUrlMatcher);
      }

      return false;
    }
  }
};
</script>
<style>
.v-list__tile--active {
  background-color: var(--secondary-color);
}
</style>
