<template>
	<h1>HomeView</h1>
	<p>{{ $route.path }}</p>
	<button class="btn btn-primary" @click="$router.push('/about')">
		About으로 이동
	</button>
</template>

<script setup>
import { useRoute } from 'vue-router';

const route = useRoute();
console.log(route.params);
</script>

<style lang="scss" scoped></style>
