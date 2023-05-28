<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-resources')"
      :mode="storedResButtonMode"
      >Stored Resources</base-button
    >
    <base-button
      @click="setSelectedTab('add-resources')"
      :mode="addResButtonMode"
      >Add Resource</base-button
    >
  </base-card>
  <KeepAlive>
    <component :is="selectedTab"></component>
  </KeepAlive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResources from './AddResources.vue';
export default {
  components: {
    StoredResources,
    AddResources,
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'offical-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation',
          link: 'https://vuejs.org',
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn how to google...',
          link: 'https://google.com',
        },
      ],
    };
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resources' ? null : 'flat';
    },
  },
  provide() {
    return {
      resources: this.storedResources,
      AddResource: this.AddResource,
      deleteResource: this.removeResource,
    };
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    AddResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
    },
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(
        (res) => res.id === resId
      );
      this.storedResources.splice(resIndex, 1);
    },
  },
};
</script>
