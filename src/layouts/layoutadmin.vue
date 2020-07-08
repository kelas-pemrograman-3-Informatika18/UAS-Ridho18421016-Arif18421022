<template>
  <q-layout view="lHh LpR lff">
    <q-header elevated>
      <q-toolbar :class="$q.dark.isActive ? 'bg-dark' : 'bg-white'" color="blue">
          <q-btn
            color="blue"
            flat
            dense
            round
            icon="menu"
            aria-label="Menu"
            @click="leftDrawerOpen = !leftDrawerOpen"
          />
          <header
          style="color: #2196f3">
            Now News
          </header>
          <q-space/>
          <header
          style="color: #2196f3">
            Admin
          </header>
        </q-toolbar>
    </q-header>
    <q-space/>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      elevated
      :class="$q.dark.isActive ? 'bg-dark' : 'bg-white'"
      >
      <q-list dark>
        <q-item-label class="bg-info" header >Navigation</q-item-label>

        <q-item
          v-for="nav in navs"
          :key="nav.label"
          :to="nav.to"
          style="color: #2196f3"
          exact
          clickable>
          <q-item-section avatar>
            <q-icon :name="nav.icon" />
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ nav.label }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script>
export default {
  name: 'MyLayout',
  data () {
    return {
      cari: null,
      leftDrawerOpen: this.$q.platform.is.desktop,
      navs: [
        {
          label: 'Data Berita',
          icon: 'assessment',
          to: '/home',
          leftDrawerOpen: false
        },
        {
          label: 'Settings',
          icon: 'settings',
          to: '/settings',
          leftDrawerOpen: false
        }
      ]
    }
  }
}
</script>
<style lang="sass" scoped>
.custom-botton
  text-align: center
  padding: 12px
  color: white
  background-color: $grey-10

</style>
<style lang="scss">
.q-drawer {
  .q-router-link--exact-active {
    color: rgb(1, 189, 251) !important;
  }
}
</style>
