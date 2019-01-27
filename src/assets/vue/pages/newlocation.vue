<template>
    <f7-page>
        <div class="row">
            <div class="col-100">
                <f7-card class="demo-card-header-pic">
                    <f7-card-header
                    class="no-border"
                    valign="bottom"><h3>Picture</h3>
                    </f7-card-header>
                    <f7-card-content>
                    <img id="location_image" style="width: 100%; height: auto;">
                    <f7-button v-on:click="takePicture" fill>Take a picture</f7-button>
                    </f7-card-content>
                </f7-card>
            </div>
            <div class="col-100">
                <f7-card class="demo-card-header-pic">
                    <f7-card-header
                    class="no-border"
                    valign="bottom"><h3>Location</h3>
                    </f7-card-header>
                    <f7-card-content>
                        <strong id="location"></strong>
                        <f7-button v-on:click="getLocation" fill>Get location</f7-button>
                    </f7-card-content>
                </f7-card>
            </div>
            <div class="col-100">
                <f7-card class="demo-card-header-pic">
                    <f7-card-header
                    class="no-border"
                    valign="bottom"><h3>Extra information</h3>
                    </f7-card-header>
                    <f7-card-content>
                        <div class="form-group">
                        <label>Name</label>
                        <input type="text" class="form-control" id="location_name" placeholder="Name of the location...">
                        </div>

                        <div class="form-group">
                        <label>Description</label>
                        <textarea cols="3" type="text" class="form-control" id="location_description" placeholder="Please fill in a cool description"></textarea>
                        </div>
                    </f7-card-content>
                </f7-card>
            </div>
        </div>
        <f7-fab position="right-bottom" slot="fixed" color="orange" v-on:click="addLocation">
            <f7-icon ios="f7:save" md="material:save"></f7-icon>
        </f7-fab>
    </f7-page>
</template>
<script>

export default {
  name: 'NewLocation',
  data () {
    return {
        bing_key: 'ArEogk_doSHMG1g0RFKb6ximvwnkZBceVj73A9aCHWUrUJTDU1Lzsd4ApWX02ufm',
        geoLocationFailed: false,
        testadrr: ''
    }
  },
    methods: {
        getLocation(){
            let gpsOptions = {maximumAge: 300000, timeout: 10000, enableHighAccuracy: true};
            if (navigator.geolocation) {
                // Get position
                navigator.geolocation.getCurrentPosition((position) => {
                    // Do request for address
                    this.$f7.request.get('http://dev.virtualearth.net/REST/v1/Locations/'
                    + position.coords.latitude + "," + position.coords.longitude +
                    '?o=json&key=' + this.bing_key, function (data) {
                        let response = JSON.parse(data);
                        let address = response.resourceSets[0].resources[0].address.formattedAddress;
                        if (address.length > 0){
                            document.getElementById("location").innerHTML = address;
                        }
                    });
                    }, (error) => { alert(error.message) }, gpsOptions)
                }
        },
        takePicture(){
            let cameraOptions =  {
                quality: 100,
                destinationType: Camera.DestinationType.FILE_URI,
                sourceType: Camera.PictureSourceType.CAMERA
                }

            navigator.camera.getPicture(this.cameraSuccess, this.cameraError, cameraOptions);
        },
        cameraSuccess(imageURI){
            document.getElementById("location_image").src = imageURI;
        },
        cameraError(){
            alert('Capturing picture failed');
        },
        addLocation(){
            let location = document.getElementById("location").innerHTML;
            let location_name = document.getElementById("location_name").value;
            let location_description = document.getElementById("location_description").value;
            let location_picture = document.getElementById("location_image").src;
            
            let newLocation = {name: location_name, location: location, 
            description: location_description, picture: location_picture};

            // Get location from localStorage
            if (localStorage.getItem('locations') === null){
                localStorage.setItem('locations', '[]');
            }

            let locations = JSON.parse(localStorage.getItem('locations'));

            locations.push(newLocation);
            localStorage.setItem('locations', JSON.stringify(locations));

            // Go to homepage
            this.$f7router.navigate("/");
        }
    }
}

</script>
<style scoped>
.page{
    background-color: #e0e0e0;
}

input{
    width: 100%;
}

textarea{
    width: 100%;
}
</style>