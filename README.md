# three-geo-map

<p>
  <a href="https://www.npmjs.com/package/three-geo-map"><img alt="npm version" src="https://img.shields.io/npm/v/three-geo-map"></a>
</p>


A JavaScript library to build 3D maps with [three.js](https://threejs.org).


## About

[three-geo-map](https://github.com/WhidRubeld/three-geo-map) takes two slippy map tilesets, one to fetch elevation data tiles, the other to texture the meshes built from said elevation data (any XYZ tileserver will do).


## Installation

```bash
yarn add three three-geo-map
```


## Quick start

```typescript
import { Scene } from 'three'
import { Map, Source } from 'three-geo-map'

const scene = new Scene()

const location = { lat: 45.916216, lon: 6.860973 }
const source = new Source({ api: 'eox' })
const map = new Map({ source, location })

scene.add(map)
map.init(() => {
  console.log('Map is ready')
})
```
[**See full example**](https://github.com/WhidRubeld/three-geo-map/tree/master/examples/basic)


## Documentation

### `Source` class

Defines a tileset source used to fetch textures applied to the 3D terrain mesh.

```typescript
const source = new Source({ api, token })
```

| Argument | Description | Required | Default Value |
| -------- | ----------- | -------- | ------------- |
| api | One of `['osm', 'mapbox', 'eox', 'maptiler']` | `true` | - |
| token | Your api key | for `mapbox` or `maptiler` | - |

### `Map` class

The main class three-geo-map. Creates a 3D map using a grid of tiles.

```javascript
const map = new Map({ source, location, options, material })
```

| Argument | Type | Description | Required | Default Value |
| -------- | ----------- | ----------- | -------- | ------------- |
| source | `Source` | A source instance | `true` | - |
| position | `{ lat: number, lon: number }` | An object containing the latitude and longitude values used to center the map | `true` | - |
| options | `MapOptions` | An object to pass some map options (override) | `false` | `defaultMapOptions` |
| material | `QuadTextureMaterialOptions` | An object to pass some material options (override) | `false` | `defaultTextureOptions` |

#### The `MapOptions` type

| Option | Type | Description |
| -------- | ----------- | ----------- |
| nTiles | `number` | `Map.init()` will display a grid of `nTiles x nTiles`
| zoom | `number` | Default zoom level
| tileSize | `number` | Tile size at the default zoom level
| tileSegments | `number` | Number of segments given to the [PlaneGeometry](https://threejs.org/docs/#api/en/geometries/PlaneGeometry) constructor. Maximum value is `256`
| zScale | `number` | The raw elevation data is multiplied by zScale when building a Tile mesh

#### The `defaultMapOptions` object
```typescript
{
  nTiles: 3,
  zoom: 11,
  tileSize: 600,
  tileSegments: 100,
  zScale: 0.045
}
```

#### The `QuadTextureMaterialOptions` type

These props are passed to [ShaderMaterial](https://threejs.org/docs/#api/en/materials/ShaderMaterial).

| Option | Type | Description |
| -------- | ----------- | ----------- |
| lights | `boolean` | Defines whether this material uses lighting; true to pass uniform data related to lighting to this shader
| wireframe | `boolean` | Render geometry as wireframe (using GL_LINES instead of GL_TRIANGLES)
| fog | `boolean` | Define whether the material color is affected by global fog settings; true to pass fog uniforms to the shader

#### The `defaultTextureOptions` object
```typescript
{
  lights: true,
  wireframe: false,
  fog: true
}
```

### Map helpers

#### Determine the location of a point by coordinates
```typescript
const { x, y, z } = map.getPosition({ lat: 45.916216, lon: 6.860973, alt: 1027 }, { loadTile: true })
```

| Argument | Type | Description | Required | Default Value |
| -------- | ----------- | ----------- | -------- | ------------- |
| location | `{ lat: number, lon: number, alt: number }` | Your geolocation | `true` | - |
| options | `{ loadTile: number }` | Extra options (override) | `false` | `{ loadTile: true }` |

Parameter `loadTile` determines whether to load the required tile if you have specified coordinates that are outside the already loaded tiles.

## Future list
- [ ] Asynchronous analog of the `getPosition` function without passing the altitude parameter
- [ ] Base Pin component
- [ ] Base Direction component
- [ ] React example
- [ ] Polyfill installation tutorial (for Webpack 5)

## License and attributions

The map library code is MIT licensed.

<p>This project is an updated and more advanced version of the <a href="https://github.com/blaze33/map33.js">map33.js</a> library.</p>
