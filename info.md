	<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js" integrity="sha512-+NqPlbbtM1QqiK8ZAo4Yrj2c4lNQoGv8P79DPtKzj++l5jnN39rHA/xsqn8zE9l0uSoxaCdrOgFs6yjyfbBxSg==" crossorigin="anonymous"></script>
	<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
	<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/js/bootstrap.min.js" integrity="sha384-+YQ4JLhjyBLPDQt//I+STsc9iw4uQqACwlvpslubQzn4u2UU2UFM80nGisd026JF" crossorigin="anonymous"></script>
	<script src="https://unpkg.com/deck.gl@latest/dist/dist.dev.js"></script>
	<!-- optional if mapbox base map is needed -->
	<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.js'></script>
	<link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.css' rel='stylesheet' />
	<script src='ressources/js/script.js'></script>

const ICON_MAPPING = {
  marker: {
    x: 0,
    y: 0,
    width: 128,
    height: 128,
    mask: true
  }
};

const data = [{
  name: 'Colma (COLM)',
  address: '365 D Street, Colma CA 94014',
  exits: 4214,
  coordinates: [-122.466233, 37.684638]
}]

const deckgl = new deck.DeckGL({
  container: 'deckglmap',
  mapStyle: 'https://basemaps.cartocdn.com/gl/positron-nolabels-gl-style/style.json',
  initialViewState: {
    longitude: -122.45,
    latitude: 37.8,
    zoom: 15
  },
  controller: true,
  layers: [
    new deck.IconLayer({
      id: 'icon-layer',
      data,
      pickable: true,
      // iconAtlas and iconMapping are required
      // getIcon: return a string
      iconAtlas: 'https://raw.githubusercontent.com/visgl/deck.gl-data/master/website/icon-atlas.png',
      iconMapping: ICON_MAPPING,
      getIcon: d => 'marker',

      sizeScale: 15,
      getPosition: d => d.coordinates,
      getSize: d => 5,
      getColor: d => [Math.sqrt(d.exits), 140, 0]
    }),
    new deck.TextLayer({
      id: 'text-layer',
      data,
      pickable: true,
      getPosition: d => d.coordinates,
      getText: d => d.name,
      getSize: 32,
      getAngle: 0,
      getTextAnchor: 'middle',
      getAlignmentBaseline: 'center',
      backgroundColor: [255, 255, 255],
      sizeMinPixels: 100
    })
  ],
  getTooltip: object => object.object ? `${object.object.name}\n${object.object.address}` : null

});

setTimeout(() => {
  deckgl.setProps({
    initialViewState: {
      longitude: 0,
      latitude: 0,
      zoom: 10,
      transitionInterpolator: new deck.FlyToInterpolator({speed: 3}),
      transitionDuration: 'auto'
    }
  })
}, 5000)
