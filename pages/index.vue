<template>
  <v-flex fill-height>
    <no-ssr>
      <l-map :zoom="zoom" @update:center="centerUpdated" :center="center">
        <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"></l-tile-layer>
        <l-control-attribution position="bottomright" :prefix="`${zoom}`"></l-control-attribution>

        <div :id="index" :key="index + 'V'" v-for="(vehicles, index) in routeV">
          <l-circle-marker 
          :key="bus.id" 
          v-for="bus in vehicles.items" 
          :color="getColorFromRouteNumber(bus.route_id)" 
          :lat-lng="[bus.latitude, bus.longitude]">
            <l-popup>
              Heading: {{getHeadingFromNumber(bus.heading)}} <br/>
              Seconds Since Last Report: {{bus.seconds_since_report}} <br/>
              Train ID: {{bus.id}}
            </l-popup>
          </l-circle-marker>
        </div>

        <div :id="index" :key="index + 'S'" v-for="(stops, index) in routeS">
          <l-circle-marker 
          :key="stop.id" 
          v-for="stop in stops.items" 
          color="black"
          :radius="4"
          :lat-lng="[stop.latitude, stop.longitude]">
              <station-tooltip :stop="stop" :route-id="stops.route_id"></station-tooltip>
          </l-circle-marker>
        </div>
        <!-- <l-marker :lat-lng="[47.413220, -1.219482]"></l-marker> -->
      </l-map>
    </no-ssr>
  </v-flex>
</template>

<script>
import StationTooltip from '../components/stationTooltip'
export default {
  components: {
    "station-tooltip": StationTooltip
  },
  transition: {
        name: 'about',
        enterToClass: "animated zoomIn",
        enterActiveClass: "animated zoomIn",
        leaveActiveClass: "animated zoomOutDown"
  },
  data() {
    return {
      zoom: 9,
      routeS: null,
      routeV: null,
      center: [34.220677699999996, -118.6323018],
      interval: null
    }
  },
  async asyncData({$axios})
  {
    let routeV = []
    let routeS = []

    routeV.push(await $axios.$get("http://api.metro.net/agencies/lametro/routes/901/vehicles/"))
    for(let x = 1; x < 7; x++)
    {
      routeV.push(await $axios.$get(`http://api.metro.net/agencies/lametro-rail/routes/80${x}/vehicles/`))
    }

    let stop = await $axios.$get("http://api.metro.net/agencies/lametro/routes/901/stops/")
    stop.route_id = "901"
    routeS.push(stop)

    for(let x = 1; x < 7; x++)
    {
      let stop = await $axios.$get(`http://api.metro.net/agencies/lametro-rail/routes/80${x}/stops/`)
      stop.route_id = `${800 + x}`
      routeS.push(stop)
    }

    return {routeS, routeV}
  },
  created()
  {
    this.$nuxt.$on("refresh-locations", async () => {this.routeV = await this.getRouteVInfo()})
    this.interval = setInterval(() => this.$nuxt.$emit("refresh-locations"), 1000*60)
  },
  beforeDestroy()
  {
    this.$nuxt.$off("refresh-locations")
    clearInterval(this.interval)
  },
  methods: {
    centerUpdated(center)
    {
      this.center = center
    },
    getHeadingFromNumber(heading)
    {
      var val = Math.floor((heading / 22.5) + 0.5);
      var arr = [
        "North",
        "North by North East", 
        "North East", 
        "East by North East", 
        "East", 
        "East by South East", 
        "South East", 
        "South by South East", 
        "South", 
        "South by South West", 
        "South West", 
        "West by South West", 
        "West", 
        "West By North West", 
        "North West", 
        "North by North West"
      ];
      return arr[(val % 16)];
    },
    getColorFromRouteNumber(routeNum)
    {
      switch(routeNum)
      {
        case "901":
          return "orange"
        case "801":
          return "blue"
        case "802":
          return "red"
        case "803":
          return "green"
        case "804":
          return "gold"
        case "805":
          return "#8A2BE2"
        case "806":
          return "aqua"
      }
    },
    async getRouteVInfo()
    {
      let routes = []
      routes.push(await this.$axios.$get("http://api.metro.net/agencies/lametro/routes/901/vehicles/"))
      for(let x = 1; x < 7; x++)
      {
        routes.push(await this.$axios.$get(`http://api.metro.net/agencies/lametro-rail/routes/80${x}/vehicles/`))
      }
      return routes
    },
    async getRouteSInfo()
    {
      let routes = []
      routes.push(await this.$axios.$get("http://api.metro.net/agencies/lametro/routes/901/stops/"))
      for(let x = 1; x < 7; x++)
      {
        routes.push(await this.$axios.$get(`http://api.metro.net/agencies/lametro-rail/routes/80${x}/stops/`))
      }
      return routes
    }
  }
};
</script>

<style>
</style>
