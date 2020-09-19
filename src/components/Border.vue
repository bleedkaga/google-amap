<template>
	<div class="map" ref="map"></div>
</template>

<script>
import json from './border.json';
// import json1 from '../data/20200818.json';
// import json2 from '../data/20200819.json';
const locations = [
    // ...json1,
    // ...json2
    ...json
]
export default {
	name: 'HelloWorld',
	props: {
		msg: String,
    },
    data(){
        return {
            locations: locations.map(item => ({ lat: Number(item.lat), lng: Number(item.lng)}))
        }
    },
	mounted() {
		this.loadSDK();
	},
	methods: {
		loadSDK() {
			return new Promise((resolve) => {
				const el = document.getElementById('google-map');
				if (!window.google && !el) {
					const script = document.createElement('script');
					script.src =
						'https://maps.googleapis.com/maps/api/js?key=AIzaSyCV2d9LzYSbwjyuJlCZheIojJTOHg6zwXA&language=en&libraries=drawing,geometry,visualization,places';
					script.id = 'google-map';
					document
						.getElementsByTagName('head')[0]
						.appendChild(script);
					script.onload = () => {
						resolve();
						console.log('resolve');
						this.checkMap();
					};
				} else {
					this.checkMap();
					resolve();
				}
			});
        },
        checkMap(){
            this.initGoogleMap()
        },
		async initGoogleMap(opts) {
            if(!window.google){
                await this.loadSDK()
            }
            if(!this.map){
                const options = {
                    center:  { lat: -33.871, lng: 151.197 },
                    zoom: 12,
                    minZoom: 4,
                    disableDefaultUI: true,
                    ...opts
                }
                this.map = new google.maps.Map(this.$refs.map, options)
                this.addMarker()
                // this.addMarkerCluster();
                this.fitBounds();
                // this.addPolygon()
                // this.addPolyline();
            }
        },
        fitBounds(){
            const bounds = new google.maps.LatLngBounds()
            this.locations.forEach(item => {
                bounds.extend(
                    new google.maps.LatLng(
                        Number(item.lat),
                        Number(item.lng)
                    )
                )
            })
            this.map.fitBounds(bounds)
        },
        addMarker(){
            // var uluru = {lat: -25.344, lng: 131.036};
            this.locations.forEach(item => {
                new google.maps.Marker({position: item, map: this.map, icon: '/p.png'});
            })
        },
        addMarkerCluster(){
            var labels = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
            var markers = this.locations.map(function(location, i) {
                return new google.maps.Marker({
                    position: location,
                    label: labels[i % labels.length]
                });
            })
            new MarkerClusterer(this.map, markers,
            {imagePath: 'https://webapi.amap.com/theme/v1.3/markers/n/mark_bs.png'});
        },
        addPolyline(){
            const flightPlanCoordinates = this.locations.map(item => ({ lat: Number(item.lat), lng: Number(item.lng)}));
            const flightPath = new google.maps.Polygon({
                path: flightPlanCoordinates,
                geodesic: true,
                strokeColor: "rgba(255,255,0,0.4)",
                strokeOpacity: 0.5,
                strokeWeight: 2
            });
            flightPath.setMap(this.map)
        },
        addPolygon(){
            const bermudaTriangle = new google.maps.Polygon({
                paths: this.locations,
                strokeColor: "#FF0000",
                strokeOpacity: 0.8,
                strokeWeight: 2,
                fillColor: "#FF0000",
                fillOpacity: 0.35
            });
            bermudaTriangle.setMap(this.map);
        }
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
	margin: 40px 0 0;
}
ul {
	list-style-type: none;
	padding: 0;
}
li {
	display: inline-block;
	margin: 0 10px;
}
a {
	color: #42b983;
}
.map {
	height: 100vh;
	background: #f8f8f8;
}
</style>
