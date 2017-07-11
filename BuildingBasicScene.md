# Building a Basic Scene

https://aframe.io/docs/0.6.0/guides/building-a-basic-scene.html

## Starting with HTML

Minimal HTML structure:

```html
<html>
  <head>
    <!-- import A-Frame minified script -->
    <script src="https://aframe.io/releases/0.6.0/aframe.min.js"></script>
  </head>
  <body>
    <!-- a-scene will contain every entity in our scene -->
    <a-scene>
    </a-scene>
  </body>
</html>
```

## Adding an Entity

Within the `<a-scene>`, we add 3D entities using one of A-Frame's standard primitives.

The primitive [`<a-box>`](https://aframe.io/docs/0.6.0/primitives/a-box.html) can be used just like a HTML tag and be given certain attributes.
 
a-box documentation: https://aframe.io/docs/0.6.0/primitives/a-box.html

```html
<a-scene>
    <a-box color="red"></a-box>
</a-scene>
```

Primitives are A-Frame's easy-to-use HTML elements that wrap the underlying entity-component assembly.

Underneath `<a-box>` is `<a-entity>` with the `geometry` and `material` components.

```html
<a-entity id="box" geometry="primitive: box" material="color: red"></a-entity>
```

## Transforming an Entity in 3D

A-Frame uses a right-handed coordinate system.

* (thumb) X-axis points right,
* (index finger) Y-axis points up,
* (middle finger) Z-axis points out of the screen, toward us.

A-Frame's distance unit is in meters.

A-Frame's rotational unit is in degrees.

Each one of these properties can receive 3 values separated by spaces (x y z)

* position
* rotation
* scale

### Parent and Child Transforms

Child entities inherit transformations (position, rotation, scale) from their parent.

### Placing our Box in Front of the Camera

All entities have their initial position as (0 0 0), including the camera.

So you might need to move your objects away from the camera to make them visible.

```html
<a-scene>
  <a-box color="red" position="0 2 -5" rotation="0 45 45" scale="2 2 2"></a-box>
</a-scene>
```

### Default controls

For computers the default control is `click-drag` the mouse pointer, or `WASD` or arrow keys.

On mobile, it uses the phone's accelerometer to move the camera's position.

## Adding a Background to the Scene

`<a-sky>` completely surrounds the scene. It's a reversed sphere surrounding the scene, and we can apply any material to the sky, like images, videos, or just color.

## Applying an Image Texture

We can apply an image texture to the box with an image, video or `<canvas>` using the `src` attribute.

```html
<a-scene>
  <a-box src="https://i.imgur.com/mYmmbrp.jpg" position="0 2 -5" rotation="0 45 45"
    scale="2 2 2"></a-box>
  <a-sky color="#222"></a-sky>
</a-scene>
```

### Using the Asset Management System

https://aframe.io/docs/0.6.0/core/asset-management-system.html

It's recommended to use the Asset Management System for performance, because it caches the assets, and A-Frame will make sure all of the assets are fetched before rendering.

* Add `<a-assets>` to the scene
* Put the `<img>` assets under `<a-assets>`
* Give the `<img>` an HTML `id`.
* Reference the asset using the ID in DOM selector format.

```html
<a-scene>
  <a-assets>
    <img id="boxTexture" src="https://i.imgur.com/mYmmbrp.jpg">
  </a-assets>
  <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2"></a-box>
  <a-sky color="#222"></a-sky>
</a-scene>
```

### Adding a Background Image

We can use an image texture to get a 360 background image by using `src` in `<a-sky>`.

```html
<a-scene>
  <a-assets>
    <img id="boxTexture" src="https://i.imgur.com/mYmmbrp.jpg">
    <img id="skyTexture"
      src="https://cdn.aframe.io/360-image-gallery-boilerplate/img/sechelt.jpg">
  </a-assets>
  <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2"></a-box>
  <a-sky src="#skyTexture"></a-sky>
</a-scene>
```

## Adding a Ground