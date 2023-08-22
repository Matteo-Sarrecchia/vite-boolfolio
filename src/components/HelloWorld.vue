<script >
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data: function () {
    return {
      projects: [],
      pages: []
    }
  },
  methods: {
    loadPage(target) {
      if (target == null) return; this.loadProjects(target);
    },
    loadProjects(target) {
      axios.get(target)
        .then(response => {
          const data = response.data;
          console.log(data); this.projects = data.projects.data;
          this.pages = data.projects.links;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted() {
    this.loadProjects('http://127.0.0.1:8000/api/v1/project-index');
  }
}
</script>


<template>
  <div class="container">
    <h1>Projects</h1>
    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div class=" card" v-for="project in projects" :key="project.id">
        [{{ project.id }}] {{ project.title }}

      </div>
    </div>






    <div class="pages row justify-content-center cursor-pointer py-5">
      <div v-for="(page, index) in pages" :key="index" class="page col " :class="(page.active ? 'bg-white text-dark' : 'bg-secondary')
        + ' '
        + (page.url == null ? 'd-none' : '')" v-html="page.label" role="button" @click="loadPage(page.url)">
      </div>
    </div>
  </div>
</template>