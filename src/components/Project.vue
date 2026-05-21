<template>
	<!-- My Projects -->
            <section class="pb-5" id="projects">
                    <h1 class="mt-5 mb-5 mx-4 pb-4 text-start px-3" id="projects">Projects</h1>

                    <div
                      v-for="(group, index) in chunkedProjects"
                      :key="index"
                      class="row justify-content-center my-4"
                    >

                	<ProjectCard
        				v-for="project in group"
        				:key="project.id"
        				:project="project"
        			/>
                    	
                    </div>
                

            </section>
</template>

<script setup>
	import {computed} from 'vue';

	import ProjectCard from './ProjectCard.vue';

	import projects from '../data/projects.json'

	const chunkSize = 3;

	const chunkedProjects = computed(() => {
		const chunks = [];

		for (let i = 0; i < projects.length; i += chunkSize) {
			chunks.push(projects.slice(i, i + chunkSize))
		}
		return chunks;
	})
</script>