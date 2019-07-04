# mapbox-introduction
This is a short introduction of mapbox.

For more information on mapbox, head over to the [official documentation of Mapbox](https://docs.mapbox.com/mapbox-gl-js/api/).

## Displaying your map
Let's start by displaying a map on our website. In order to do that, include the JavaScript and CSS files in the <head> of your HTML file.
  
```
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.js'></script>
<link href='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.css' rel='stylesheet' />
```

Include the following code in the <body> of your HTML file.

```
<div id='map' style='width: 400px; height: 300px;'></div>
<script>
mapboxgl.accessToken = 'pk.eyJ1IjoiYW5nbWluc2hlbmciLCJhIjoiY2pydDhjMjlwMXhpaDN5cHMxcjNya2ZmbyJ9.Tc5kmo0vZ1VKJbLK83OloA';
var map = new mapboxgl.Map({
container: 'map',
style: 'mapbox://styles/mapbox/streets-v9'
});
</script>
```

## Instructions

This exercise will help you to:
- Practice MApbox API integration with an app
- Add location properties in your models as GeoJSON
- Display content from the database in a map

![cofee-book pic](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_141038aa0f5ce10c722722400bfdc6d5.jpg)

