<template>
	<div class="col-md-4 mb-4 d-flex justify-content-center">
		<div class="card projects-card h-100">

			<img :src="project.image" class="card-img-top card-img" alt="...">

			<div class="card-body">
			<h5 class="card-title">{{project.title}}</h5>
			<p class="card-text">
			   {{project.description}}
			</p>
			
			</div>
			
		</div>
                    
    </div>
</template>

<script setup>
	import {defineProps} from 'vue';

	//define the props
	defineProps({
		project: Object
	})
</script>