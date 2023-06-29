<template>
  <div>
      <input
        v-model="addressInput"
        @input="searchAddress"
        type="text"
        placeholder="Enter your address"
      />
      <ul v-if="addressResults.length">
        <li v-for="(result, index) in addressResults" :key="index" @click="selectAddress(result)">
          {{ result.label }}
        </li>
      </ul>
    </div>
  <HelloWorld :longitude="longitude" :latitude="latitude"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios';
  
export default {
  components: {
    HelloWorld
  },
  data() {
    return {
      addressInput: '',
      addressResults: [],
      longitude: 0,  // Ajout de la propriété longitude
      latitude: 0,   // Ajout de la propriété latitude
    };
  },
  methods: {
    async searchAddress() {
      if (this.addressInput.length < 4 || !/\w{4}/.test(this.addressInput)) {
        this.addressResults = [];
        return;
      }

      try {
        const response = await axios.get(
          `https://api-adresse.data.gouv.fr/search/?q=${encodeURIComponent(this.addressInput)}&limit=5`
        );
        
        const features = response.data.features;
        const addresses = features.map((feature) => ({ label: feature.properties.label, coordinates: feature.geometry.coordinates }));
        console.log(addresses);
        this.addressResults = addresses;
      } catch (error) {
        console.error(error);
      }
    },
    selectAddress(result) {
      this.addressInput = result.label;
      this.addressResults = [];
      this.longitude = result.coordinates[0];
      this.latitude = result.coordinates[1]; 
    }
  }
};
</script>


<style>
#app {
    position: relative;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    overflow: hidden;
    
  }
  Html {
    background-color: rgb(233, 230, 230);
  }

</style>
