<template>
    <div>
    </div>
</template>

<script>
export default {
    name: 'LocationHandler',
    data() {
        return {
            currentLocation: {
                lat: 51.2,
                lng: 4.1,
            }
        }
    },

    mounted() {
        this.getLocation();
    },

    methods: {
        async getLocation() {
            let data = this;
            return new Promise((resolve, reject) => {

                if(!("geolocation" in navigator)) {
                    reject(new Error('Geolocation is not available.'));
                }

                navigator.geolocation.getCurrentPosition(pos => {
                    data.showPosition(pos);
                }, err => {
                   data.positionError(err)
                });

            });
        },
        showPosition(position) {
            this.currentLocation = {
                lat: position.coords.latitude,
                lng: position.coords.longitude,
            };
            this.$store.commit('setCurrentLocation', this.currentLocation);
        },
        positionError(error) {
            this.$emit('hasGeoAccessFromUser', error);
        },
    }
       
}
</script>
