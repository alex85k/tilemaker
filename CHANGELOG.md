# Changelog

## [1.2.0] - 2015-10-08

### Added
- Load shapefiles into layers
- Spatial queries (Intersects, FindIntersecting) on shapefiles
- Choose deflate, gzip or no compression (@tinoue)
- Show trace on Luabind errors (@tinoue)

### Changed
- Suppress "missing way" errors unless --verbose specified

### Fixed
- Die less horribly on Lua syntax errors
- Don't add attributes if no Layer set
- AttributeNumeric error (@tinoue)

## [1.1.0] - 2015-09-28

### Added
- `LayerAsCentroid` method to write centroid of polygons (for labelling and POIs)
- Option: simplify geometries on output
- Option: `write_to` combines multiple input layers in one output layer
- Option: gzip output compression
- Option: user-specified output metadata
- Vagrant config to ease creating VMs (@zerebubuth)
- Compile sources to .o temporary files, for faster recompiles (@zerebubuth)
- Lua/JSON config for Mapbox GL style-compatible output (@flamed0011)
- Simple Ruby .mbtiles server for testing

### Changed
- Store nodes with projected latitudes
- 10% speedup by using unordered_map (@zerebubuth)

### Fixed
- Don't die when `keys_vals` is empty in source .pbf (fixes bbbike/metro compatibility)
- Code correctness, esp. avoiding reallocating arrays (@zerebubuth)
- Build improvements (@zerebubuth)
- Documentation improvements

## [1.0.0] - 2015-06-29

### Added
- Initial release
