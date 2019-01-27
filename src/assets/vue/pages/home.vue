<template>
    <f7-page>
        <div class="row" v-for="location in locations">
        <!-- Each "cell" has col-[width in percents] class -->
            <div class="col-100">
                <f7-card class="demo-card-header-pic">
                    <f7-card-header class="no-border"><h2>{{location.name}}</h2> <span class="deleteSpan" v-on:click="deleteLocation(location)">delete</span></f7-card-header>
                    <f7-card-content>
                        <div class="row">
                            <div class="col-30"><img class="imgthumb" :src="location.picture"></div>
                            <div class="col-70"><p>{{location.location}}: <br>{{location.description}}</p></div>
                        </div>
                    </f7-card-content>
                </f7-card>
            </div>
        </div>
        <f7-fab position="right-bottom" slot="fixed" color="orange" href="/newlocation">
            <f7-icon ios="f7:add" md="material:add"></f7-icon>
        </f7-fab>
    </f7-page>
</template>
<script>
export default {
  name: 'Home',
  data () {
    return {
        locations: JSON.parse(localStorage.getItem('locations'))
    }
  },
  methods: {
      deleteLocation(location){
          for (var i = 0; i < this.locations.length; i++){
              if (this.locations[i].picture === location.picture){
                  this.locations.splice(i, 1);
                  localStorage.setItem('locations', JSON.stringify(this.locations));
              }
          }
      }
  }
}

</script>

<style scoped>
.imgthumb{
    height: 60px !important;
    width: 60px !important;
}

.page{
    background-color: #e0e0e0;
}

.deleteSpan{
    color: red;
    font-size: 12px;
    font-weight: bold;
}
</style>
