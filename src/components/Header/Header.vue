<!-- Header Structure -->
<template>
	<div>
		<v-navigation-drawer
			app
			fixed
			v-model="drawer"
			:width="250"
			class="nknx-sidebar background-none"
			:right="rtlLayout"
		>
			<!-- App Sidebar -->
			<app-sidebar></app-sidebar>
		</v-navigation-drawer>
		<v-toolbar 
			class="nknx-toolbar"
			app
			fixed
		>
			<div class="d-custom-flex align-items-center navbar-left">
				<div>
					<v-toolbar-side-icon icon large @click.stop="drawer = !drawer" class="v-step-0"></v-toolbar-side-icon>
				</div>
				<mobile-search-form></mobile-search-form>
				<v-form style="flex: auto;" @submit.prevent="search" class='search-form'>
					<v-text-field  flat solo prepend-icon="search" v-model.trim="searchString" :placeholder="$t('message.search')" class="search-bar "></v-text-field>
				 </v-form>
			</div>

			<div class="navbar-right">
				<v-btn v-if="!$auth.check()" color="primary" tag="link" :to="'/login'">{{ $t('message.login') }}</v-btn>
				<notifications v-if="$auth.check()"></notifications>
				<user v-if="$auth.check()"></user>
				<language-provider></language-provider>
			</div>
		</v-toolbar>
	</div>
</template>

<script>
import User from "./User";
import LanguageProvider from "./LanguageProvider";
import MobileSearchForm from "./MobileSearchForm";
import Notifications from "./Notifications";
import { mapGetters } from "vuex";
import Sidebar from "../Sidebar/Sidebar.vue";

export default {
  data() {
    return {
			searchString:"",
      collapsed: false, // collapse sidebar
      drawer: true, // sidebar drawer default true
      sidebarImages: "", // sidebar background images
      enableDefaultSidebar: false
    };
	},
  computed: {
    ...mapGetters([
      "rtlLayout",
			"activeHeaderFilter"
    ])
  },
  methods: {
		search(){
			if (this.searchString[0] == "N" && this.searchString.length === 34){
				this.$router.push({ path: '/address/'+this.searchString})
			} 
			else if(this.searchString.length > 34){
				this.$router.push({ path: '/block/hash/'+this.searchString})
			}
			else{
				this.$router.push({ path: '/block/height/'+this.searchString})
			}
		},
  },
  components: {
    LanguageProvider,
		MobileSearchForm,
		User,
		Notifications,
		appSidebar: Sidebar,
  }
};
</script>