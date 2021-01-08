<template>
<v-app>
<v-navigation-drawer 
v-model="drawer" :clipped="clipped" fixed app>
<v-list>
<v-list-item>
<v-list-item-content>
<img src="/logo.png" alt="ssep login" style="width:50px; height:auto">
</v-list-item-content>
</v-list-item>

<v-list-item v-for="(item, i) in menus" :key="i" :to="item.to" router exact>
<v-list-item-action>
<v-icon style="border-radius:50%; padding:7px;" class="secondary white--text">{{ item.icon }}</v-icon>
</v-list-item-action>
<v-list-item-content>
<v-list-item-title  v-text="item.title"/>
</v-list-item-content>
</v-list-item>
</v-list>
</v-navigation-drawer>

<v-app-bar app class="secondary">
<v-app-bar-nav-icon @click.stop="drawer = !drawer" class="white--text"/>
<!-- {{title}} -->
<span class="white--text" v-if="this.$auth.user" > Welcome, <b>{{this.$auth.user.name}}</b></span>
<v-spacer />
<v-icon class="white--text" @click="logout">{{ logout_btn.icon }}</v-icon>
<!-- <span v-if="this.$auth.user" > Welcome, <b>{{this.$auth.user.name}}</b> -->
<!-- <v-btn text> -->
<!-- {{logout_btn.label}} -->

<!-- </v-btn> -->
<!-- </span> -->
</v-app-bar>
<v-main>

<v-container>
<nuxt />
</v-container>

</v-main>

<v-footer :fixed="fixed" app class="primary white--text">
<span>&copy; {{year}}</span>
</v-footer>
</v-app>
</template>

<script>
export default {

data () {
return {
year: new Date().getFullYear(),
clipped: false,
fixed:false,
drawer: true,
menus : [],      

miniVariant: false,
right: true,
rightDrawer: false,
title: 'Virtual School',
logout_btn:{
icon:'mdi-logout',
label:'Logout'
}
}
},
created () {
  this.get_menus();
},
methods:{
async logout() {
  await this.$auth.logout();
},
get_menus () {

  var menu = [
            { icon: 'mdi-apps', title: 'Home',to: '/' },
            { icon: 'mdi-chart-bubble', title: 'User',to: '/user' },
            { icon: 'mdi-chart-bubble', title: 'Quick SMS',to: '/quick_sms' },
            { icon: 'mdi-chart-bubble', title: 'Bulk SMS',to: '/bulk_sms' },
            { icon: 'mdi-chart-bubble', title: 'Contact',to: '/contact' },
            { icon: 'mdi-chart-bubble', title: 'Campaign',to: '/campaign' },
            { icon: 'mdi-chart-bubble', title: 'City',to: '/city' },
            
  ];

  this.menus = menu;

  



}
}
}
</script>

<style>
/* .grad {
  background: linear-gradient(to bottom right, orange, grey);
} */
</style>
