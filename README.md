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

Create a new project with `irongenerate` to start the lab.

![wonder pic](https://images.unsplash.com/photo-1551171129-8ce1ebb911b3?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80)

### Iteration 1 - Displaying your map

Let's start by displaying a map on our website. In order to do that, include the JavaScript and CSS files in the `<head>` of your `layout.hbs`.
  
```html
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.js'></script>
<link href='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.css' rel='stylesheet' />
```


Include the following code in the <body> of the hbs file where you want to display the map, for example `index.hbs`.

```html
<div id='map' style='width: 85vw; height: 60vh; margin:50px auto;'></div>

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
    - imageUrl,
    - timestamps 
```

### Iteration 3 - Updating our `Place` model

[GeoJSON](https://mongoosejs.com/docs/geojson.html) is a format for storing geographic points and polygons. [MongoDB has excellent support for geospatial queries on GeoJSON objects](http://thecodebarbarian.com/80-20-guide-to-mongodb-geospatial-queries). Let's take a look at how you can use Mongoose to store and query GeoJSON objects.

Since this might be the first time we are working with GeoJSON Objects in mongoose, let do this together.
After referring to the offical documentation, this is how our `Place` model should look like, where `location` is a point.

```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const placeSchema = new Schema({
  name: String,
  imageUrl: String,
  location: {
    type: {
      type: String,
      enum: ['Point'],
      default: 'Point'
    },
    coordinates: {
      type: [Number],
      required: true
    }
  }
}, {
    timestamps: {
      createdAt: 'created_at',
      updatedAt: 'updated_at'
    }
  }
)

const Place = mongoose.model('Place', placeSchema)

module.exports = Place
```

### Iteration 3 - Creating CRUD operation for our model

In this iteration, we will create full CRUD on `place.js` model - to be able to create, update, delete and display all the places you save in the database. You will have to create all the required routes and the corresponding views.

Let's start with the `create` operation. In your `index.hbs`, create a form with a `POST` method to add new places into our database.

```js
<form action="/add-wonder" method="POST">

  <label for="name">name:</label>
  <input type="text" name="name">
  <label for="imageUrl">image-url:</label>
  <input type="text" name="imageUrl">
  <label for="longitude">latitude:</label>
  <input type="number" step="0.0001" name="latitude">
  <label for="longitude">longitude:</label>
  <input type="number" step="0.0001" name="longitude">
  <button>add wonder</button>
</form>
```

After you have the input form, fill the database with the information of the wonders that can be found [here](https://en.wikipedia.org/wiki/Seven_Wonders_of_the_Ancient_World#Wonders).

The `update` and `delete` route can be added but will not be discussed in this lab. 

At the end of the iteration, you should now have a form and the map in your `index.hbs`.

### Iteration 5 - Passing data from mongodb to the client with Response.json()

Now that the new places is saved to the database, it is time to display them on the map.
Any idea on how to do that?

In order to display the information of the database on the map which is on the client side javascript, we need to pass the data from the server side to the client side.

We can send JSON to the client by using `Response.json()`, a useful method.

It accepts an object or array, and converts it to JSON before sending it:

```js
res.json({ name: 'Ben' })
```

The complete GET route will look something like this:

```js
router.get('/api/places', (req, res, next) => {
  Place.find({}).then(places=>{
  res.json(places)
  })
})
```

Head over to `localhost:3000/api/places` and you will see your data in the form of JSON object.

### Iteration 6 - Displaying the coordinate of the places on our map.

Now comes the most exciting part of the introduction. From the last iteration we now have access to the places stored in the database. It's time to display them on the map.

We start with doing an axios.get request on the client side to obtain the JSON object data.

Next, let's take another quick look at the [official documentation on marker.](https://docs.mapbox.com/mapbox-gl-js/api/#marker)

```js
let marker = new mapboxgl.Marker()
  .setLngLat([30.5, 50.5])
  .addTo(map)
```

This is all we need to display a marker on the map. At the end of the iteration, the client side javascript will look like this:

```js
 axios.get('http://localhost:3000/api/places').then((response) => {
    let places = response.data

    places.forEach(place => {
      let marker = new mapboxgl.Marker()
        .setLngLat(place.location.coordinate)
        .addTo(map)
    })
  })
  ```
  
  ### Bonus iteration - Attach a popup to a marker instance in mapbox
  
Mapbox has many build in component. Other than the marker component, the [popup component](https://docs.mapbox.com/mapbox-gl-js/example/set-popup/) is another commonly used component. In this bonus iteration, let's display a popup on click of our markers on the map.

```js
axios.get('http://localhost:3000/api/places').then((response) => {
    let places = response.data
    
    places.forEach(place => {
      let popup = new mapboxgl.Popup({ offset: 0, className: 'my-class' })
        .setHTML(`<div class="popup"><p>${place.name}</p><img src="${place.imageUrl}" alt=""></div>`)
        .setMaxWidth("none")

      let marker = new mapboxgl.Marker()
        .setLngLat(place.location.coordinate)
        .setPopup(popup)
        .addTo(map)
    })
  })
  
  ```

  
  
  
  
That's it! Now you have successfully created your first mapbox project! Mapbox is very powerful and offer much more than this. Head over to the official documentation to learn more about it.

Happy coding! 💗💗💗💗💗💗💗💗💗







