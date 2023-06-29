
<template>
	<div class="card">
		<div class="nav">
			
			<h1 class="settings-title">Search</h1>
		</div>
		<div class="body">
			<div class="search">
				<a>
					<input
						type="text"
						class="search-bar"
						placeholder="Search dishes, restaurants"
						v-model="searchQuery"
					/>

				</a>
        
			</div>
			<div class="keywords">
				<h1 class="title-keywords">All Categories</h1>
				<ChipsTag
					v-for="category in categories"
					:key="category.name"
					:text="category.name"
					:selected="selectedCategories"
					@select="selectCategory"
				/>

			</div>
			<SuggestionCard
			v-for="(restaurant, index) in restaurants"
			:key="index"
			:restaurant="restaurant"
			></SuggestionCard>

		</div>
	</div>
</template>
<script>
/* eslint-disable */
import ChipsTag from "@/components/ChipsTag.vue";
import SuggestionCard from "@/components/SuggestionCard.vue";
import { computed, ref, onMounted } from 'vue';
import axios from 'axios';

export default {
	props: {
		longitude: Float64Array,
    latitude: Float64Array,
	},
	setup(props) {

		const restaurants = ref([]);
		const categories = ref([]);
		const selectedCategories = ref([]);
		const searchQuery = ref('');

		const fetchRestaurants = async () => {
			try {
				const longitude =props.latitude;
				const latitude = props.longitude;
				const response = await axios.get(`http://3.83.173.28:5000/api/menus/get/list/nearby?longitude=${longitude}&latitude=${latitude}`);
        console.log(response);
				restaurants.value = response.data;

				restaurants.value.forEach(restaurant => {
					const restaurantCategories = restaurant.description.split(' - ');
					categories.value = [...categories.value, ...restaurantCategories];
				});

				categories.value = [...new Set(categories.value.map(category => category.toLowerCase()))];

				categories.value = categories.value.map(category => ({ name: category, selected: true, delete: false }));

				console.log(categories.value);
			} catch (error) {
				console.error(error);
			}
		};

		onMounted(fetchRestaurants);

		const filteredRestaurants = computed(() => {
			let result = restaurants.value;

			if (selectedCategories.value.length > 0) {
				result = result.filter(restaurant =>
				restaurant.description.split(' - ').some(rCategory =>
					selectedCategories.value.some(sCategory =>
					rCategory.trim().toLowerCase() === sCategory.toLowerCase()
					)
				)
				);
			}

			if (searchQuery.value) {
				result = result.filter(restaurant =>
				restaurant.fullname.toLowerCase().includes(searchQuery.value.toLowerCase())
				);
			}

			return result;
		});

		const filteredChips = computed(() => {
			if (selectedCategories.value.length === 0) {
				return categories.value;
			} else {
				return categories.value.filter(category =>
				selectedCategories.value.includes(category.name)
				);
			}
		});

		const selectCategory = (categoryName) => {
			const index = selectedCategories.value.indexOf(categoryName);
			if (index > -1) {
				selectedCategories.value.splice(index, 1);
			} else {
				selectedCategories.value.push(categoryName);
			}
		};

		return { restaurants: filteredRestaurants, categories: filteredChips, selectCategory, searchQuery }; 
	},
	components: { ChipsTag,SuggestionCard },
	data() {
		return {
			delete: true,
		};
	},
};

</script>

<style scoped>
/* .body {
	display: flex;
    overflow: auto;
    padding-top: 0;
	overflow-x: hidden;
	margin-left: 20px;
	margin-right: 20px;
	border-radius: 10px ;
}
.search,
.title-div,
.categories,
.title-section {
	margin-right: auto;
}
.search::before {
	content: url("@/assets/search.svg"); 
	position: relative;
	pointer-events: none;
	left: -40px;
	top: 43px;
	border-radius: 10px ;
}
.search {
	margin: 0;
	width: 95%;
	border-radius: 10px ;
}
.search-bar {
	padding-left: 20px;
	padding-right: 8px;
	margin: 0;
	width: 100%;
	border-radius: 10px ;

} */

.search,
.title-div,
.categories,
.title-section {
	margin-right: auto;
}
.search::before {
	content: url("@/assets/search.svg"); /* Replace with your icon path */
	position: relative;
	pointer-events: none;
	top: 40px;
	left: 20px;
}
.search-bar {
	width: 86vw;
	padding-left: 50px;

	max-width: 660px;
}
@media (max-width: 555px){
	.search-bar {
		width: 78vw;
	}
}
.body {
	display: flex;
    overflow: auto;
    flex-wrap: nowrap;
    justify-content: space-around;
    padding: 20px;
    text-align: left;
    padding-top: 0;
    align-items: flex-start;
    align-content: flex-start;
    overflow-x: hidden;
    flex-direction: column;
}
.card {
	max-width: 750px;
	min-width: 350px;
	width: 100%;
	margin: auto;
	background: #ffffff;
	border-radius: 25px;
	padding-bottom: 5px;
	display: block;
	overflow: auto;
}
.settings-title {
	padding-left: 13px;
}
.nav {
	display: flex;
	justify-content: flex-start;
	align-items: center;
	margin-left: 20px;
	padding-top: 20px;
}

h1 {
	font-family: "Sen";
	font-style: normal;
	font-weight: 400;
	font-size: 17px;
	line-height: 22px;
	color: #181c2e;
}
.title-keywords {
	font-family: "Sen";
	font-style: normal;
	font-weight: 400;
	font-size: 20px;
	line-height: 24px;
	/* identical to box height */

	text-transform: capitalize;

	color: #32343e;
}
.keywords {
	margin-right: auto;
}
.chip {
    display: inline-flex;
    justify-content: space-between;
    padding: 10px;
    border: 2px solid #EDEDED;
	border-radius: 33px;
    font-size: 14px;

    margin: 5px;
}

.chip span {
    font-family: 'Sen';
font-style: normal;
font-weight: 400;
font-size: 16px;
line-height: 19px;
letter-spacing: -0.333333px;

color: #181C2E;
}

.delete-btn {
    background: none;
    border: none;
    cursor: pointer;
}
.image-restaurant {
	width: 60px;
	height: 50px;
}
.suggestion-card {
	display: flex;
    justify-content: flex-start;
    align-content: center;
    align-items: center;
    margin: 10px 0 10px 0;
}
.image {
	width: 100%;
	height: 100%;
	border-radius: 8px;
}
.description-restaurant {
	display: flex;
    flex-direction: column;
    align-content: flex-start;
    align-items: flex-start;
}
.title {
	

font-family: 'Sen';
font-style: normal;
font-weight: 400;
font-size: 16px;
line-height: 19px;
letter-spacing: -0.333333px;

color: #32343E;
margin: 0;
padding-left: 10px;
}
.description {
	font-family: 'Sen';
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 17px;
    text-transform: capitalize;
    color: #A0A5BA;
    margin-top: 2px;
	padding-left: 10px;
}
.search-bar-routerlink {
	text-decoration: none;
}
input {
    display: flex;
    background: #f0f5fa;
    border-radius: 10px;
    height: 4em;
    font-family: "Sen";
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 17px;
    border: 0;
    align-items: center;
    width: 291px;
    color: #a0a5ba;
    margin-bottom: 8px;
}
</style>
