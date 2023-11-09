<template>
	<div>
		<h2>{{ form.title }}</h2>
		<p>{{ form.content }}</p>
		<p class="text-auted">{{ form.createdAt }}</p>
		<hr class="my-4" />
		<div class="row g-2">
			<div class="col-auto">
				<button class="btn btn-outline-dark">이전글</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-dark">다음글</button>
			</div>
			<div class="col-auto me-auto"></div>
			<div class="col-auto">
				<button class="btn btn-outline-dark" @click="goListPage">목록</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-primary" @click="goEditPage">
					수정
				</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-danger" @click="deletePage">삭제</button>
			</div>
		</div>
	</div>
</template>

<script setup>
import { deletePost, getPostById } from '@/api/posts';
// import { useRoute } from 'vue-router';
import { useRouter } from 'vue-router';
import { ref } from 'vue';

// const route = useRoute();
const router = useRouter();
const form = ref({});

const props = defineProps({
	id: String,
});

// const id = route.params.id;

const goListPage = () => {
	router.push({
		name: 'PostList',
	});
};

const fetchPost = async () => {
	const { data } = await getPostById(props.id);
	setPost(data);
};
const setPost = ({ title, content, createAt }) => {
	form.value.title = title;
	form.value.content = content;
	form.value.createAt = createAt;
};
fetchPost();

const deletePage = async () => {
	try {
		await deletePost(props.id);
		router.push({ name: 'PostList' });
	} catch (error) {
		console.log(error);
	}
};

const goEditPage = () => {
	router.push({
		name: 'PostEdit',
		params: {
			id: props.id,
		},
	});
};
</script>

<style lang="scss" scoped></style>
