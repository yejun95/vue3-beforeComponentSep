<template>
	<div>
		<h2>게시글 목록</h2>
		<hr class="my-4" />
		<form @submit.prevent>
			<div class="row g-3 justify-content-center">
				<div class="col">
					<input v-model="params.title_like" type="text" class="form-control" />
				</div>
				<div class="col-3">
					<select v-model="params._limit" class="form-select">
						<option value="3">3개씩 보기</option>
						<option value="6">6개씩 보기</option>
						<option value="9">9개씩 보기</option>
					</select>
				</div>
			</div>
		</form>
		<hr class="my-4" />
		<div class="row g-3">
			<div v-for="post in posts" :key="post.id" class="col-4">
				<PostItem
					:title="post.title"
					:content="post.content"
					:createdAt="post.createdAt"
					@click="goPage(post.id)"
				></PostItem>
			</div>
		</div>
		<nav class="my-4" aria-label="Page navigation example">
			<ul class="pagination justify-content-center">
				<li class="page-item">
					<a
						class="page-link"
						href="#"
						aria-label="Previous"
						@click.prevent="params._page--"
						:class="{ disabled: !(params._page > 1) }"
					>
						<span aria-hidden="true">&laquo;</span>
					</a>
				</li>
				<li
					class="page-item"
					v-for="page in pageCount"
					:key="page"
					:class="{ active: params._page === page }"
				>
					<a class="page-link" href="#" @click.prevent="params._page = page">{{
						page
					}}</a>
				</li>
				<li class="page-item">
					<a
						class="page-link"
						href="#"
						aria-label="Next"
						@click.prevent="params._page++"
						:class="{ disabled: !(params._page < pageCount) }"
					>
						<span aria-hidden="true">&raquo;</span>
					</a>
				</li>
			</ul>
		</nav>
		<hr class="my-4" />
		<AppCard>
			<PostDetailView :id="1"></PostDetailView>
		</AppCard>
	</div>
</template>

<script setup>
import PostItem from '@/components/posts/PostItem.vue';
import PostDetailView from '@/views/posts/PostDetailView.vue';
import AppCard from '@/components/AppCard.vue';
import { ref, computed, watchEffect } from 'vue';
import { getPosts } from '@/api/posts';
import { useRouter } from 'vue-router';
// import { useRouter } from 'vue-router';

const posts = ref([]);
const router = useRouter();
const params = ref({
	_sort: 'createdAt',
	_order: 'desc',
	_limit: '3',
	_page: 1,
	title_like: '',
});
// pagination
const totalCount = ref(0);
const pageCount = computed(() =>
	Math.ceil(totalCount.value / params.value._limit),
);

const fetchPost = async () => {
	try {
		const { data, headers } = await getPosts(params.value);
		posts.value = data;
		totalCount.value = headers['x-total-count'];
	} catch (error) {
		console.log(error);
	}
};
watchEffect(fetchPost);

const goPage = id => {
	router.push({
		name: 'PostDetail',
		params: {
			id,
		},
	});
};
</script>

<style lang="scss" scoped></style>
