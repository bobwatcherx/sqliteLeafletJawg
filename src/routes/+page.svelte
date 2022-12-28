<script>
	import L from 'leaflet'
	import 'leaflet/dist/leaflet.css'
	import {onMount} from 'svelte'

const accessToken = "SbJ1xnaUApVYSuGhKgnadoHczoP2m4SzpSF95vwwuipRXc4n92UlEcd58qrMqKHa"

// SET STYLE OF MAPS YOU
const styles = [
	"jawg-streets",
	"jawg-sunny",
	"jawg-terrain",
	"jawg-dark",
	"jawg-light"
	]

const mycustomLayer = {};



onMount(()=>{
	let latlng = [0,0]
	// 3 IS ZOOM
	var mymap = L.map("mymaps").setView(latlng,3)

	// CREATE TILE LAYER
	styles.forEach((style)=>{
		mycustomLayer[style] = L.tileLayer(`https://tile.jawg.io/${style}/{z}/{x}/{y}{r}.png?access-token=${accessToken}`,{
		maxZoom:22
	})
	})

	// SET DEFAULT IF OPEN THE MAPS FIRST
	mycustomLayer["jawg-streets"].addTo(mymap)

	// ADD CONTROLL IN TOP RIGHT FOR SWITCH THEME MAPS
	L.control.layers(mycustomLayer).addTo(mymap)
	

	// DETECT YOU CURRENT POSITION
navigator.geolocation.getCurrentPosition(location=>{
	latlng = new L.latLng(location.coords.latitude,location.coords.longitude)
	// ADD FLY IF YOU LOCATION IS DETECT
	mymap.flyTo(latlng,15,{
		animate:true,
		duration:0.5

	})
	// CREATE RADIUS CIRCLE ON MARKER
	L.circle(latlng,{radius:300}).addTo(mymap)

	// CREATE MARKER YOU LOCATION
	L.marker(latlng).addTo(mymap).bindPopup("You Here Guys").openPopup()

},
// IF ERROR FOR DETECT LOCATION SEND CONSOLE ERORR
error=>console.log(error),
// HIGH ACURACY YOU DETECT LOCATION
{enableHighAccuracy:true}
)

})
</script>

maps 
<div id="mymaps" style="height:500px;width:700px"></div>