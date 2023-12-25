<template>
  <div class="home">
    <div v-if="projects.length">
      <FilterNav @filterChange=" current = $event" :current="current"/>
      <div v-for="project in filteredProjects" :key="project.id">
      <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
    <div v-else>

    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProjct.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'HomeView',
  data(){
  return{
    projects: [],
    current: 'all'
  }
  },
  components: {
    SingleProject,
    FilterNav
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err))
  },
  methods: {
    handleDelete(id){
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handleComplete(id){
      let p = this.projects.find(project => {
        return project.id === id
      })
      p.complete = !p.complete
    }
  },
  computed: {
    filteredProjects(){
      if(this.current === 'completed'){
        return this.projects.filter(p => p.complete)
      }
      if(this.current === 'ongoing'){
        return this.projects.filter(p => !p.complete)
      }
      else{
        return this.projects
      }
    }
  }
}
</script>
