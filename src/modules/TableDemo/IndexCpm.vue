<template>
  <div id="table" class="container">
    <h1>{{title}}</h1>
    <ul id="navs" class="nav nav-pills" role="tablist">
      <template v-for='(item,index) in urlItems'>
        <li :key="index" role="presentation" v-if="item.isDropdown != true">
          <router-link :to="item.linkTo">{{ item.name }}</router-link>
        </li>
        <li :key="index" role="presentation" class="dropdown" v-else>
          <router-link class="dropdown-toggle" data-toggle="dropdown" :to="item.linkTo">
            {{ item.name }} <span class="caret"></span>
          </router-link>
          <ul class="dropdown-menu" role="menu">
            <li :key="cindex" v-for="(u,cindex) in item.children">
              <router-link :to="item.linkTo + u.linkTo">{{ u.name }}</router-link>
            </li>
          </ul>
        </li>
      </template>
    </ul>
    <transition :name="transitionName">
      <router-view keep-alive></router-view>
    </transition>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'table',
  data () {
    return {
      transitionName: ''
    }
  },
  computed: {
    ...mapState(['title', 'urlItems'])
  },
  watch: {
    '$route' (to, from) {
      const toDepth = to.path.split('/').length
      const fromDepth = from.path.split('/').length
      this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
    }
  }
}
</script>
