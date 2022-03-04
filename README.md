# Mapbox TouchDesigner 

This TouchDesigner component renders a mapbox GL JS webview and provides mechanisms for panning, zooming, and changing the map bearing.

## Usage

This component is provided as a .tox file located at [release/mapbox.tox](release/mapbox.tox). To use include the tox into your patch and fill in the Access Token field on Mabbox page with your Mapbox Access Token which should be avilable on your [mapbox account](https://account.mapbox.com/) page.

It is recommended that you click the Save Token to Disk pulse button after entering your Access Token as this will prevent you from saving the Access Token in your patch.

You may also set your map styling on the Mapbox page as well as manually reload the web view. The Port parameter is the port that is used to host the webview and websocket that is used to communicate with the web view. If you have more than one Mapbox component running at a time each component will need it's own port.

On the Output page output background color and resolution may be set.

On the Map Target page you may set where you would like the map to center on and what zoom level and bearing. You may also toggle the Animate to Target toggle to have the mapbox api animate changes made to the target values.

The Map Status page provides you info on the current status of the map, including it's center lat lng, zoom, bearing, and bounds. The Loaded flag indicates that the load event in mapbox gl has triggered. The Map Timeout flag indicate that the connection with the webview has failed and that you may want to trigger the Reset Map pulse on the Mapbox page.

There are two inputs to the component. 

__in_tracked_projections__ - This should be a table with the following columns name, lat, lng. Each the lat and lng values will be translated to x and y coordinates relative to the current map view upper corner. These vales will be available in the component output dat.

__in_extensions_js__ - The contents of this DAT will be loaded as Javascript into the web view.




### Python API

Public usable Python method for the Mapbox TD component.

### UpdateTargetFromActual()
Copy the actual map configuration to the target map configuration.

Users may manipulate the map view via means other than modifying the target map values, via ZoomToBounds or a 
direct call to Communicate. After these manipulations the target values may not match what is present in map.
So if a change is made to the target values you may see jerkey behavior. This method allows you to set the 
target to the current reported state of the map.


### ZoomToBounds(lng1, lat1, lng2, lat2, ease=None, easeOptions=None, options={'padding':50} )
Zoom the map to show the entirety of a bounding box.

:param lng1: Number Top left longitude of the bounding box.
:param lat1: Number Top left latitude of the bounding box.
:param lng2: Number Bottom right longitude of the bounding box.
:param lat2: Number Bottom right latitude of the bounding box.
:param ease: (optional) Bool Should the action ease to the selected bounds. Will use the value of the paramater 
Targetanimate if not provided.
:param easeOptions: (optional) Dictionary of animation options to pass to the easeTo map method.
see: https://docs.mapbox.com/mapbox-gl-js/api/map/#map#easeto
:param options: (optional) Dictionary of options to pass to the showBounds method
by default {'padding':50}
see: https://docs.mapbox.com/mapbox-gl-js/api/map/#map#cameraforbounds


### ResetWebOp()
Reload the webview via turning off and on the active paramater.


### ClearTrackedProjections
Stop tracking all tracked projection


### TrackProjection(name,location)
Track a new projection. 

Tracked projections take a latitude and longitude and converst them to a x y coordinate relative to the 
top left of the map output texture. Values will be updated when the map positioning/zoom changes.

WARNNING: do not use this call directly if you are also using the input table method for tracking projections.
Changing the input table stops tracking all previously tracked projections.

:param name: name of the projection to be tracked
:param location: a LatLng like can be list eg: [-118.04,34.8] or dictionary eg: {'lat':34.8,'lng':-118.04} 
NOTE: in list format it is [lng, lat]


### UnTrackProjection(name)
Stop tracking a tracked projection by name.

:param name: the name of the projection to stop tracking.

### AddGeoJSONSource(sourceId, geojson)
Adds a GeoJSON source to the map. Sources can then be used in new Layers.

If a source with the same sourceId already exists this method will update the data in that source.

:param sourceId: String ID for the new or existing layer.
:param geojson: GeoJSON as a python dictionarty structure.



### RemoveGeoJSONSource(sourceId)
Removes a GeoJSON Source from the map.

WARNING: you must first remove all Layers that use the source before removing the source. Sources with layers
depending on the source will not be removed.

:param sourceId: string ID of the source to be removed

### AddGeoJSONLayer(layerId, sourceId, layerStyles, beforeId=None)
Add a Layer to the map.

If a layer with the same layerId already exists this method will update the options for that layer.

:param layerId: String ID of the layer being added/updated. 
:param sourceId: String ID of the source that the Layer will use for it's source geometry.
:param layerStyles: Layer configuration information. Python dictionary with layer configuration information. 
see https://docs.mapbox.com/mapbox-gl-js/api/map/#map#addlayer This should exclude the id, and source fields.


### RemoveGeoJSONLayer(layerId)
Removes a Layer from the map.

:param layerId: The string ID of th eLayer to remove.

### Communicate(obj)
Send a object to the map page via the web socket.

This method is intended for advanced usage.


## Authors

- UCLA REMAP
- Isaac Gierard [github.com/igierard](https://github.com/igierard)