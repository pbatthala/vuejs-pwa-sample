<template>
  <Header>
    <template slot="header-content">
      <md-field md-clearable class="md-menu">
        <md-input placeholder="Search" v-model="keyword" @keyup.enter="search" />
       </md-field>
       <md-button class="md-icon-button" @click="search">
          <md-icon>search</md-icon>
        </md-button>
    </template>
    
    <template slot="app-content">
      <Card v-for="content in contents" 
          :title="content.title" 
          :content="content.description" 
          :url="content.event_url"
          :key="content.event_id"
          :update="update" />
      <md-progress-spinner v-if="update" md-mode="indeterminate"></md-progress-spinner>
    </template>
  </Header>
</template>

<script>
import Header from '../components/Header.vue'
import Card from '../components/Card.vue'
 
export default {
  name: 'home',
  data: () => ({
    keyword: '',
    contents: [],
    update: false,
  }),
  components: {
    'Card': Card,
    'Header': Header,
  },
  methods: {
    search() {
      this.update = true
      this.$jsonp('https://connpass.com/api/v1/event/', {keyword: this.keyword}).then(json => {
            this.update = false
            this.contents = json.events
        }).catch(err => {
            console.log(err)
        })
    }
  }
}
</script>

<style>
input {
    color: white !important; 
}
input::placeholder {
    color: white !important; 
}
</style>