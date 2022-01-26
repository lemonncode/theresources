<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-resources')"
      :mode="storedResButtonMode"
    >
      Stored Resource</base-button
    >
    <base-button
      @click="setSelectedTab('add-resource')"
      :mode="addResButtonMode"
    >
      Add Resource</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoredResources,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'stored-resources',

      storedResources: [
        {
          id: 'official-guide',
          title: 'Oficial Guide',
          description: 'The official Vuejs documentation',
          link: 'https://vuejs.org',
        },
        {
          id: 'google',
          title: ' Google',
          description: 'Learn to Google...',
          link: 'https://google.org',
        },
        {
          id: 'udemy',
          title: 'Udemy',
          description: 'Udemy Academy',
          link: 'https://udemy.org',
        },
      ],
    };
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.deleteResource,
    };
  },
  computed: {
    storedResButtonMode() {
      return this.selecTab === 'stored-resource' ? null : 'flat';
    },
    addResButtonMode() {
      return this.selecTab === 'add-resource' ? null : 'flat';
    },
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
      };
      this.storedResources.push(newResource);
      this.selectedTab = 'stored-resources';
    },
    deleteResource(resId) {
      const resIndex = this.storedResources.findIndex(res => res.id === resId);
      this.storedResources.splice(resIndex, 1);
    },
  },
};
</script>