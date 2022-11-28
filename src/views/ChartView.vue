<template>
  <div class="home">
    <apexchart 
      width="580" 
      type="donut" 
      :options="options" 
      :series="series"
      :logout="logout"
    ></apexchart>
  </div>
  <button type="button" class="btn btn-primary" @click="logout">Log Out</button>
</template>

<script>
// @ is an alias to /src
import { ref, onMounted } from "vue";

export default {
    name: 'ChartView',
    setup() {
        const options = ref({});
        const series = ref([44, 55, 41, 17, 15]);

        const logout = function() {
          localStorage.removeItem('user');
          location.reload()
        }

        onMounted(async () => {
          // var response = await fetch("/api/bookings/aggregate/groupby");

          let token = localStorage.getItem("user");

          var response = await fetch("/api/bookings/aggregate/groupby", {
              headers: {
                "x-access-token": token
              }
          });

          if (response.ok) {
              var heroes = await response.json();

              series.value = heroes.map(a => a.count);
              options.value = { labels: heroes.map(a => a._id) };
          }
        });

        return {
            options, 
            series,
            logout,
        };
    },
};
</script>
