<template>
  <div class="regions" id="app">
    <button
    class="regions__btn"
    @click="getData"
    >Регионы
    </button>
    <hr>
      <ul :key="i" v-for = "(region, i) in regions">
        <li @click="createArrCity(region);">
          {{region}}
          <div v-if="activeRegion === region">
            <ul :key="j" v-for = "(city, j) in cities">
              <li>
                {{city}}
              </li>
            </ul>
          </div>
        </li>
      </ul>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      apiTrucks: 'https://cors-anywhere.herokuapp.com/trucks.ru/api/v1/Location',
      locations: [],
      regions: [],
      cities: [],
      activeRegion: null,
    }
  },

  methods: {
    getData: function() {
       this.axios
      .get(this.apiTrucks)
      .then(response => (this.locations = response.data.city));
      this.regions = this.createArrRegions(this.locations);
    },
    createArrRegions: function(data){
      const arr = [];
        data.forEach(element => {
        if (!arr.includes(element.region_id)) {

        arr.push(element.region_id);
      }
       });
       return arr;
   },
   createArrCity: function (region) {
    this.activeRegion = this.activeRegion === region ? null : region; //тоггл(если активный регион = регион, то он будет null и наоборот, с каждым кликом по нему он меняет статус)
    const citiesArr = [];
    this.locations.forEach(element => {
        if (element.region_id === region) {
        citiesArr.push(element.city_name);
      }
       });
    return this.cities = citiesArr;
   }
    },
    }


</script>

<style lang="scss">

</style>
