<template>
    <div class="flex justify-center">
		<a-carousel
			class="w-[90%] h-100"
			arrows
			autoplay={true}
			dots-class="slick-dots slick-thumb"
			draggable
		>
			<template #prevArrow>
				<div
					class="custom-slick-arrow bg-transparent"
					style="left: 50px"
				>
				<a-button >
					<LeftOutlined  style="margin-bottom:10px;"/>
				  </a-button>
					
				</div>
			</template>
			<template #nextArrow>
				<div
					class="custom-slick-arrow bg-transparent"
					style="right: 50px"
				>
				<a-button >
					<RightOutlined />
				  </a-button>
					
				</div>
			</template>
                <a-image
				v-for="item in data"	
				:src="apiURL.URL + item.post_image[0].image_path"
				:preview="{ visible: false }"
				@click="visible = true"
				:previewMask="false"
			/>	
		</a-carousel>   
	</div>
</template>

<script setup>
import { LeftOutlined, RightOutlined } from '@ant-design/icons-vue';
import listPostsAPI from "../../../api/posts/index";
import { ref, computed, onMounted } from "vue";
import { useRouter } from "vue-router";
import apiURL from "../../../api/constants";

const props = defineProps({
    title: {
        type: String,
        default: null,
    },
    condition: {
        type: String,
        default: null,
    },
});

const imagesData = ref('');
const data = ref();
const filter = ref({
    "min_price": 0,
    "max_price": 60000000000,
    "min_area": 0,
    "max_area": 1000,
    "dirs": [
        0,
        1,
        2,
        3,
        4,
        5,
        6,
        7,
        8
    ],
    "address": "",
    "sold_status": [],
    "priority_status": []
})

const fetchPostsList = async (filter, page = 1, pageSize = 10) => {
    data.value =[];
	let res;

	res = await listPostsAPI.getPostByFilter({
		...filter,
		page: page,
		pageSize: pageSize,
	});
    data.value = res.data;
    console.log(data.value);
}

onMounted(() => {
      fetchPostsList();

});
</script>

<script>
export default {};
</script>

<style scoped>
</style>
