<template>
  
    <div class="suggestion-card">
      <routerLink :to="`/clients/restaurant/${restaurant.fullname}`">
      <div class="image-restaurant">
        <img class="image" :src="restaurant.image" alt="Avatar" />
      </div>
    </routerLink>
      <div class="description-restaurant">
        <h1 class="title">{{ restaurant.fullname }}</h1>
        <p class="description">{{ restaurant.description }}</p>
      </div>
    </div>
  </template>
  
  <script>
  /* eslint-disable */
  export default {
    name: "SuggestionCard",
    props: {
      restaurant: Object,
    },
  };
  </script>


<style scoped>
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
</style>
