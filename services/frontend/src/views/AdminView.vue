<template>
  <v-container fluid>
    <!-- Movie Selection Dropdown -->
    <v-row>
      <v-col>
        <!-- Movie Selection Dropdown -->
        <v-select
          v-model="selectedMovie"
          :items="movies"
          item-title="name"
          item-value="_id"
          label="Select a movie"
          @change="setSelectedMovie"
          return-object
        ></v-select>
      </v-col>
    </v-row>

    <!-- Movie Details Card and Orders List -->
    <v-row>
      <v-col cols="4">
        <v-card>
          <v-card-title>Movie Details</v-card-title>
          <v-card-text>
            <div v-if="selectedMovie">
              <p><strong>Title:</strong> {{ selectedMovie.name }}</p>
              <p><strong>Room:</strong> {{ selectedMovie.room }}</p>
              <p><strong>Date:</strong> {{ selectedMovie.datetime }}</p>
              <!-- Add more movie details here -->
            </div>
          </v-card-text>
        </v-card>
      </v-col>

      <v-col cols="8">
        <v-card class="overflow-y-auto" max-height="400">
          <v-card-title>Orders</v-card-title>
          <v-card-text>
            <v-list>
              <v-list-item
                v-for="(order, index) in orders"
                :key="index"
                @click="selectOrder(order)"
                :class="{ 'order-selected': selectedOrder === order }"
              >
                <v-list-item-content>
                  <v-list-item-title>{{
                    order.customerName
                  }}</v-list-item-title>
                  <v-list-item-subtitle>{{
                    order.orderDate
                  }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Cancel Order Button -->
    <v-row>
      <v-col>
        <v-btn v-if="selectedOrder" color="error" @click="cancelSelectedOrder">
          Cancel Selected Order
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
  
  <script>
import axios from "axios";

export default {
  data() {
    return {
      selectedMovie: null,
      movies: [
        {
          _id: "1",
          name: "Chihiros Reise ins Zauberland",
          datetime: "2023-06-14T19:30:00.000+00:00",
          room: "J007",
        },
      ],
      orders: [
        { customerName: "Customer 1", orderDate: "2020-01-01" },
        { customerName: "Customer 2", orderDate: "2020-01-02" },
      ],
      selectedOrder: null,
    };
  },
  methods: {
    getMovies() {
      // Implement logic to fetch movies from the backend
      axios
        .get("/api/v1/movies/", {
          withCredentials: false, // Ensure credentials are not sent
        })
        .then((response) => {
          // Handle success
          this.movies = response.data;
        })
        .catch((error) => {
          // Handle errors
          console.log(error);
        });
    },
    selectOrder(order) {
      if (this.selectedOrder === order) {
        this.selectedOrder = null;
      } else {
        this.selectedOrder = order;
      }
    },
    cancelOrder(order) {
      console.log(order);
    },
    cancelSelectedOrder() {
      if (this.selectedOrder) {
        // Implement logic to cancel the selected order
      }
    },
  },
  created() {
    this.getMovies();
  },
};
</script>
  
  <style scoped>
.order-selected {
  background-color: #ef605a; /* Apply a background color to selected orders */
}
</style>
  