<template>
  <div>
    <!-- page-wrapper Start-->
    <div class="page-wrapper" id="canvas-bookmark">
      <!-- Page Header Start-->
      <div class="page-main-header" :class="{ open: toggle_sidebar_var }">
        <div class="main-header-right row">
          <div class="main-header-left d-lg-none">
            <div class="logo-wrapper"><router-link :to="{ name: 'default' }"><img src="../assets/images/endless-logo.png"
                  alt=""></router-link></div>
          </div>
          <div class="mobile-sidebar">
            <div class="media-body text-right switch-sm">
              <label class="switch" v-on:click="toggle_sidebar">
                <feather type="align-left"></feather>
              </label>
            </div>
          </div>
          <div class="nav-right col">
            <ul class="nav-menus" :class="{ open: mobile_toggle }">
              <li>
                <form class="form-inline search-form">
                  <div class="form-group">
                    <input :class="{ open: mobile_search }" class="form-control-plaintext" v-on:keyup="searchTerm"
                      v-model="terms" type="text" placeholder="Search..">
                    <div :class="searchResult ? 'Typeahead-menu is-open' : 'Typeahead-menu'" v-if="search.length">
                      <div class="ProfileCard u-cf" v-for="(menuItem, index) in search.slice(0, 8)" :key="index">
                        <div class="ProfileCard-avatar header-search">
                          <feather :type="menuItem.icon"></feather>
                        </div>
                        <div class="ProfileCard-details">
                          <div class="ProfileCard-realName">
                            <span @click='removeFix()'><router-link :to="{ path: menuItem.path }" class="realname">{{
                              menuItem.title }}</router-link></span>
                          </div>
                        </div>
                      </div>
                    </div>
                    <div :class="searchResultEmpty ? 'Typeahead-menu is-open' : 'Typeahead-menu'">
                      <div class="tt-dataset tt-dataset-0">
                        <div class="EmptyMessage">
                          Your search turned up 0 results. Opps There are no result found.
                        </div>
                      </div>
                    </div>
                    <span class="d-sm-none" v-on:click="mobile_search = !mobile_search">
                      <feather type="search"></feather>
                    </span>
                  </div>
                </form>
              </li>
              <li><a href="javascript:void(0)" class="text-dark" v-on:click="toggle_fullscreen()">
                  <feather type="maximize"></feather>
                </a></li>
              <li class="onhover-dropdown"><a class="txt-dark" href="#">
                  <h6>EN</h6>
                </a>
                <ul class="language-dropdown onhover-show-div p-20">
                  <li><a href="#" data-lng="en"><i class="flag-icon flag-icon-is"></i> English</a></li>
                  <li><a href="#" data-lng="es"><i class="flag-icon flag-icon-um"></i> Spanish</a></li>
                  <li><a href="#" data-lng="pt"><i class="flag-icon flag-icon-uy"></i> Portuguese</a></li>
                  <li><a href="#" data-lng="fr"><i class="flag-icon flag-icon-nz"></i> French</a></li>
                </ul>
              </li>
              <li class="onhover-dropdown">
                <feather type="bell"></feather><span class="dot"></span>
                <ul class="notification-dropdown onhover-show-div">
                  <li>Notification <span class="badge badge-pill badge-primary pull-right">3</span></li>
                  <li>
                    <div class="media">
                      <div class="media-body">
                        <h6 class="mt-0"><span>
                            <feather class="shopping-color" type="shopping-bag"></feather>
                          </span>Your order ready for Ship..!<small class="pull-right">9:00 AM</small></h6>
                        <p class="mb-0">Lorem ipsum dolor sit amet, consectetuer.</p>
                      </div>
                    </div>
                  </li>
                  <li>
                    <div class="media">
                      <div class="media-body">
                        <h6 class="mt-0 txt-success"><span>
                            <feather class="download-color font-success" type="download"></feather>
                          </span>Download Complete<small class="pull-right">2:30 PM</small></h6>
                        <p class="mb-0">Lorem ipsum dolor sit amet, consectetuer.</p>
                      </div>
                    </div>
                  </li>
                  <li>
                    <div class="media">
                      <div class="media-body">
                        <h6 class="mt-0 txt-danger"><span>
                            <feather class="alert-color font-danger" type="alert-circle"></feather>
                          </span>250 MB trash files<small class="pull-right">5:00 PM</small></h6>
                        <p class="mb-0">Lorem ipsum dolor sit amet, consectetuer.</p>
                      </div>
                    </div>
                  </li>
                  <li class="bg-light txt-dark"><a href="#">All</a> notification</li>
                </ul>
              </li>
              <li><a href="#" v-on:click="toggle_rightsidebar">
                  <feather type="message-circle"></feather><span class="dot"></span>
                </a></li>
              <li class="onhover-dropdown">
                <div class="media align-items-center"><img class="align-self-center pull-right img-50 rounded-circle"
                    :src="getImgUrl()" alt="header-user">
                  <div class="dotted-animation"><span class="animate-circle"></span><span class="main-circle"></span>
                  </div>
                </div>
                <ul class="profile-dropdown onhover-show-div p-20">
                  <li><a href="#">
                      <feather type="user"></feather> Edit Profile
                    </a></li>
                  <li><a href="#">
                      <feather type="mail"></feather> Inbox
                    </a></li>
                  <li><a href="#">
                      <feather type="lock"></feather> Lock Screen
                    </a></li>
                  <li><a href="#">
                      <feather type="settings"></feather> Settings
                    </a></li>
                  <li><a @click="logout">
                      <feather type="log-out"></feather> Logout
                    </a></li>
                </ul>
              </li>
            </ul>
            <div class="d-lg-none mobile-toggle pull-right" v-on:click="mobile_toggle = !mobile_toggle">
              <feather type="more-horizontal"></feather>
            </div>
          </div>
        </div>
      </div>
      <!-- Page Header Ends -->
    </div>
  </div>
</template>
<script>
var body = document.getElementsByTagName("body")[0];
import { mapState } from "vuex";

export default {
  name: 'Search',
  data() {
    return {
      terms: '',
      searchResult: false,
      searchResultEmpty: false,
      toggle_sidebar_var: false,
      clicked: false,
      toggle_rightsidebar_var: false,
      rightclicked: false,
      mobile_toggle: false,
      mobile_search: false,
      search: []
    }
  },
  computed: {
    ...mapState({
      menuItems: state => state.menu.data
    })
  },
  methods: {
    getImgUrl() {
      // console.log("localStorage.getItem('Userinfo').photoURL", localStorage.getItem('Userinfo'));

      return (localStorage.getItem('Userinfo') ? localStorage.getItem('Userinfo').photoURL : require("../assets/images/dashboard/user.png"))
    },
    searchTerm: function () {
      // this.$store.dispatch('menu/searchTerm', this.terms)
      let items = [];
      this.terms = this.terms.toLowerCase();
      this.menuItems.filter(menuItems => {
        if (menuItems.title.toLowerCase().includes(this.terms) && menuItems.type === 'link') {
          items.push(menuItems);
        }
        if (!menuItems.children) return false
        menuItems.children.filter(subItems => {
          if (subItems.title.toLowerCase().includes(this.terms) && subItems.type === 'link') {
            subItems.icon = menuItems.icon
            items.push(subItems);
          }
          if (!subItems.children) return false
          subItems.children.filter(suSubItems => {
            if (suSubItems.title.toLowerCase().includes(this.terms)) {
              suSubItems.icon = menuItems.icon
              items.push(suSubItems);
            }
          })
        })
        this.search = items
      });
    },

    logout: function () {

      this.$router.push('/auth/login');
      localStorage.clear('user')
    },
    addFix() {
      body.classList.add("offcanvas")
      this.searchResult = true;
    },
    removeFix() {
      body.classList.remove("offcanvas")
      this.searchResult = false;
      this.terms = ''
    },
    toggle_sidebar() {
      this.toggle_sidebar_var = !this.toggle_sidebar_var,
        this.clicked = !this.toggle_sidebar_var,
        this.$emit('clicked', this.clicked)
    },
    toggle_rightsidebar() {
      this.toggle_rightsidebar_var = !this.toggle_rightsidebar_var,
        this.rightclicked = !this.toggle_rightsidebar_var,
        this.$emit('rightclicked', this.rightclicked)
    },
    toggle_fullscreen() {
      if ((document.fullScreenElement && document.fullScreenElement !== null) ||
        (!document.mozFullScreen && !document.webkitIsFullScreen)) {
        if (document.documentElement.requestFullScreen) {
          document.documentElement.requestFullScreen();
        } else if (document.documentElement.mozRequestFullScreen) {
          document.documentElement.mozRequestFullScreen();
        } else if (document.documentElement.webkitRequestFullScreen) {
          document.documentElement.webkitRequestFullScreen(Element.ALLOW_KEYBOARD_INPUT);
        }
      } else {
        if (document.cancelFullScreen) {
          document.cancelFullScreen();
        } else if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen();
        } else if (document.webkitCancelFullScreen) {
          document.webkitCancelFullScreen();
        }
      }
    }
  },
  watch: {
    search: function () {
      this.terms ? this.addFix() : this.removeFix();
      if (!this.search.length)
        this.searchResultEmpty = true;
      else
        this.searchResultEmpty = false;
    }
  }
}
</script>
