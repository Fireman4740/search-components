<template>
    <div class="chip" @click="$emit('select', text)">
      <span>{{ text }}</span>
      <button v-if="selected" class="delete-btn">x</button>
    </div>
  </template>
  
<script>
export default {
	name: "ChipsTag",
	props: {
        text: String,
        image: String,
        delete: Boolean,
        selected: Boolean,
    },

};
</script>
<style scoped>
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
</style>