<template>
  <div id="app" class="app-container">
    <main-screen></main-screen>
    <about-screen></about-screen>
    <service-list></service-list>
    <contacts></contacts>
  </div>
</template>

<script>
import MainScreenView from "./views/MainScreenView"
import AboutServiceView from "./views/AboutServiceView"
import ServiceListview from './views/ServiceListView';
import Contactsview from './views/ContactsView';
export default {
  name: 'App',
  components: {
    "main-screen": MainScreenView,
    "about-screen": AboutServiceView,
    "service-list": ServiceListview,
    "contacts": Contactsview
  },
  data() {
    return {
    }
  },
}
</script>

<style lang='scss'>
@import "styles/index.scss";
</style>
