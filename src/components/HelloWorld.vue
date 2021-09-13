<template>
  <div class="filter-container">
    <label for="filter">filter</label>
    <div class="filter-row">
      <select class="" v-model="hotel" @change="fethHotelReviews($event)">
              <option value="" disabled>Select a hotel</option>
        <option v-for="(hotel, index) in hotels" :key="index" :value="hotel">
          {{ capitalize(hotel.name) }}
        </option>
      </select>

      <input type="date" v-model="from" @change="fethHotelReviews($event)"/>
      <input type="date" v-model="to" @change="fethHotelReviews($event)"/>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      hotels: [],
      hotel: '',
      loading: false,
      from: '2020-01-01',
      to: '2020-04-01',
    };
  },

  methods: {
    async fethHotels() {
      const url = `http://localhost:8080/api/v1/hotels`;
      try {
        const response = await axios.get(url);
        const results = response.data.data;
        this.hotels = results.map((hotel) => ({
          id: hotel.id,
          name: hotel.name,
        }));
      } catch (err) {
        if (err.response) {
          // client received an error response (5xx, 4xx)
          console.log("Server Error:", err);
        } else if (err.request) {
          // client never received a response, or request never left
          console.log("Network Error:", err);
        } else {
          console.log("Client Error:", err);
        }
      }
    },
    capitalize(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
    async fethHotelReviews() {
      const url = `http://localhost:8080/api/v1/reviews/${this.hotel.id}?from=${this.from}&to=${this.to}`;
      try {
        const response = await axios.get(url);
        const results = response.data.data;
        this.hotels = results.map((hotel) => ({
          id: hotel.id,
          name: hotel.name,
        }));
      } catch (err) {
        if (err.response) {
          // client received an error response (5xx, 4xx)
          console.log("Server Error:", err);
        } else if (err.request) {
          // client never received a response, or request never left
          console.log("Network Error:", err);
        } else {
          console.log("Client Error:", err);
        }
      }
    },
  },
  mounted() {
    this.fethHotels();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.filter-container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 80%;
  align-items: center;
  .filter-row {
    display: flex;
    justify-content: space-between;
  }
}
</style>
