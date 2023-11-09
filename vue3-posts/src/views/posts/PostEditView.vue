<template>
	<div>
		<h2>게시글 수정</h2>
		<hr class="my-4" />
		<form @submit.prevent="edit">
			<div class="mb-3">
				<label for="title" class="form-label">제목</label>
				<input
					type="text"
					v-model="posts.title"
					class="form-control"
					id="title"
				/>
			</div>
			<div class="mb-3">
				<label for="content">내용</label>
				<textarea
					class="form-control"
					v-model="posts.content"
					id="content"
					rows="3"
				></textarea>
			</div>
			<div class="pt-4">
				<button
					type="button"
					class="btn btn-outline-dark me-2"
					@click="goDetailPage"
				>
					취소
				</button>
				<button class="btn btn-primary">저장</button>
			</div>
		</form>
	</div>
</template>

<script setup>
import { getPostById, updatePost } from '@/api/posts';
import { useRouter } from 'vue-router';
import { useRoute } from 'vue-router';
import { ref } from 'vue';

const router = useRouter();
const route = useRoute();
const id = route.params.id;

const posts = ref({
	title: null,
	content: null,
});

const fetchPost = async () => {
	try {
		const { data } = await getPostById(id);
		posts.value = data;
	} catch (error) {
		console.log(error);
	}
};
fetchPost();

const edit = async () => {
	try {
		await updatePost(id, { ...posts.value });
		router.push({ name: 'PostDetail', params: { id } });
	} catch (error) {
		console.log(error);
	}
};

const goDetailPage = () => {
	router.push({
		name: 'PostDetail',
		params: {
			id,
		},
	});
};
</script>

<style lang="scss" scoped></style>
