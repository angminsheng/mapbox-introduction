# Express | Mapbox - Seven wonders of the world

This is a short introduction of mapbox.

For more information on mapbox, head over to the [official documentation of Mapbox](https://docs.mapbox.com/mapbox-gl-js/api/).

## Introduction

The Seven Wonders of the Ancient World is the first known list of the most remarkable man-made creations of classical antiquity, and was based on guide-books popular among Hellenic sight-seers and only includes works located around the Mediterranean rim. The number seven was chosen because the Greeks believed it to be the representation of perfection and plenty. Many similar lists have been made, including lists for the Medieval World and the Modern World.

Our goal today is very simple, let's display all the seven wonders of the world on a map so we can visualize where they are.

## Instructions

This exercise will help you to:
- Practice MApbox API integration with an app
- Add location properties in your models as GeoJSON
- Display content from the database in a map


![wonder pic](https://images.unsplash.com/photo-1551171129-8ce1ebb911b3?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80)

### Iteration 1 - Displaying your map

Let's start by displaying a map on our website. In order to do that, include the JavaScript and CSS files in the <head> of your `layout.hbs`.
  
```
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.js'></script>
<link href='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.css' rel='stylesheet' />
```


Include the following code in the <body> of the hbs file where you want to display the map, for example `index.hbs`.

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

If everything is set up correctly, you should now see your map on your webpage!

### Iteration 2 - Creating our model

In `models` folder, create a new model `place.js`. For now, let's keep our model simple :

```bash
    - name,
    - country,
    - timestamps 
```

### Iteration 3 - Creating CRUD operation for our model

In this iteration, we will create full CRUD on `place.js` model - to be able to create, update, delete and display all the places you save in the database. You will have to create all the required routes and the corresponding views.

### Iteration 4 - Updating our `Place` model

[GeoJSON](https://mongoosejs.com/docs/geojson.html) is a format for storing geographic points and polygons. [MongoDB has excellent support for geospatial queries on GeoJSON objects](http://thecodebarbarian.com/80-20-guide-to-mongodb-geospatial-queries). Let's take a look at how you can use Mongoose to store and query GeoJSON objects.

Since this might be the first time we are working with GeoJSON Objects in mongoose, let do this together.
After referring to the offical documentation, this is how our `Place` model should look like, where `location` is a point.

```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const placeSchema = new Schema({
  name: String,
  place: {
    type: String,
    enum: ['Bookstore', 'Coffee shop']
  },
  location: {
    type: {
      String,
      enum: ['Point'],
      default: 'Point'
    },
    coordinates: {
      type: [Number],
      required: true
    }
  }
})

const Place = mongoose.model('Place', placeSchema)
```





