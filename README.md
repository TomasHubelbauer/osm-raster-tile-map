# OSM Raster Tile Map

[**DEMO**](https://tomashubelbauer.github.io/osm-raster-tile-map/)

This project demonstrates the use of raster tiles from a publicly reachable, unauthenticated Mapy.cz tile server.

I've built it for fun and as a detour from trying to figure out a frustrating problem with vector tiles from a different source.

I generally believe raster tiles are inferior to vector tiles, but boy, are they easier to work with!

- Hook up the arrow buttons for map panning
- Consider adding support for https://www.mapzen.com/projects/vector-tiles/ which is vector but has slippy tiles
- Check out https://wiki.openstreetmap.org/wiki/Downloading_data#Construct_a_URL_for_the_HTTP_API for vectors from OSM
- See if I can compress `Planet.osm` or use a portion (Czech Republic / Prague) in the repository
- Link server and tile size together so the HD one (512) and the others (256) work correctly
- Hook up `+` and `-` keys for zoom in/out at the current map center
- Persist pins & strokes in the local storage
- Put back drawing strokes on the map, for now with antialiasing multiplication, in the future smarter with double buffer
  and use https://stackoverflow.com/a/365853/2715716 to display the stroke length in km/m
  canvas or by redrawing only the stroke portion of each tile, for each tile as it renders
- Consider handling single primary and secondary clicks by carrying out the action and then reverting it if double click
  as opposed to waiting to see if double click happened and only after determining it did not carrying out the action
  (This could improve the perceived performance of the UI)
- Fix the mobile error/panning error
- Persist map configuration (longitude, latitude, zoom) after each change to restore where left off
  (Possibly combine this with refactoring the map code to a class `Map` with events like onTileLoad, onTileRender etc.)
- Implement pinch to zoom
- Improve the local storage cache so it evicts the least hit tile in favor or a new tile to persist
- Use a service worker for caching the commonly hit tiles by using a logic which eventually evicts tiles with low hits
- Try to improve the loading algorithm so that it loads from the center out like Cinema4D bucket rendering (this may not be worth it)
- Refactor the map logic to a `Map` class with events for tile load (checker board) and tile render and provide multiple renderers:
  the current canvas based one (tiles and POIs rendered on canvas)
  and an image based one (tiles and POIs CSS placed)
  (Consider making it so that the renderer may opt into loading the tiles itself so the `img` based on could use checker board background
  image and let the `img` elements load the tiles)
- Benchmark the performance of the `canvas` and `img` based solutions somehow
  (DevTools FPS profile with programmatic pan/zoom sequence with clear cache?)
- Use `OffscreenCanvas` in the tile cache if supported (right now only Chrome)
- Consider implementing a rate-limiter algorithm in the tile cache so that the GitHub Pages version cannot be abused to slam the server
  (Although this would be impossible with the `img` based renderer which loads tiles itself - and a malicious person can still abuse)
- Consider redrawing the whole canvas on each new tile load so that strokes and pins could be drawn without getting clipped
  or antialiasing multiplying up by redrawing the whole stroke when just one of the tiles changed and others already had it drawn
