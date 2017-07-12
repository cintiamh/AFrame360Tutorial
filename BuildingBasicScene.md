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

To add a ground, we can use `<a-plane>`.
By default, planes are oriented parallel to the XY axis.
So you need to rotate is to negative 90 degrees and make it bigger.

You can use texture on the ground and the repeat property.

```html
<a-assets>
  <!-- ... -->
  <img id="groundTexture" src="https://cdn.aframe.io/a-painter/images/floor.jpg">
  <!-- ... -->
</a-assets>
<!-- ... -->
<a-plane src="#groundTexture" rotation="-90 0 0" width="30" height="30" repeat="10 10"></a-plane>
<!-- ... -->
```

## Tweaking Lighting

`<a-light>`.

By default A-Frame adds an ambient light and a directional light.

Once we add lights of our own, the default lighting setup is removed.

* Ambient light: sky light.
* Point light: is like a light bulb. We can position them around the scene.

```html
<a-scene>
  <a-assets>
    <img id="boxTexture" src="https://i.imgur.com/mYmmbrp.jpg">
    <img id="skyTexture"
      src="https://cdn.aframe.io/360-image-gallery-boilerplate/img/sechelt.jpg">
    <img id="groundTexture" src="https://cdn.aframe.io/a-painter/images/floor.jpg">
  </a-assets>
  <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2"></a-box>
  <a-sky src="#skyTexture"></a-sky>
  <a-light type="ambient" color="#445451"></a-light>
  <a-light type="point" intensity="2" position="2 4 4"></a-light>
</a-scene>
```

## Adding Animation

`<a-animation>` can be deprecated in favor of a separated component.
https://github.com/ngokevin/kframe/tree/master/components/animation/.

We can place an `<a-animation>` element as a child of an entity.

```html
<a-scene>
  <a-assets>
    <img id="boxTexture" src="https://i.imgur.com/mYmmbrp.jpg">
  </a-assets>
  <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2">
    <a-animation attribute="position" to="0 2.2 -5" direction="alternate" dur="2000"
      repeat="indefinite"></a-animation>
  </a-box>
</a-scene>
```

### Advance Details

## Adding Interaction

Basic mobile and desktop inputs with cursor component.

The cursor component by default provides the ability to "click" on entities by starting or gazing at them on mobile, or on desktop, looking at an entity and click the mouse.

To have a visible cursor fixed to the camera, we place the cursor as a child of the camera.

```html
<a-scene>
  <a-assets>
    <img id="boxTexture" src="https://i.imgur.com/mYmmbrp.jpg">
  </a-assets>
  <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2">
    <a-animation attribute="position" to="0 2.2 -5" direction="alternate" dur="2000"
      repeat="indefinite"></a-animation>
  </a-box>
  <a-camera>
    <a-cursor></a-cursor>
  </a-camera>
</a-scene>
```

The cursor component:
https://aframe.io/docs/0.6.0/components/cursor.html

### Event Listener Component

One way to manually handle the cursor events is to add an event listener with JavaScript.

```javascript
var boxEl = document.querySelector('a-box');
boxEl.addEventListener('mouseenter', function () {
    boxEl.setAttribute('scale', {x: 2, y: 2, z: 2});
});
```

We can also include the logic inside an A-Frame component:

```html
<script>
  AFRAME.registerComponent('scale-on-mouseenter', {
    schema: {
      to: {default: '2.5 2.5 2.5'}
    },
    init: function () {
      var data = this.data;
      this.el.addEventListener('mouseenter', function () {
        this.setAttribute('scale', data.to);
      });
    }
  });
</script>
```

Or

```html
<script>
  AFRAME.registerComponent('scale-on-mouseenter', {
    // ...
  });
</script>
<a-scene>
  <!-- ... -->
  <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2"
         scale-on-mouseenter="to: 2.2 2.2 2.2">
    <a-animation attribute="position" to="0 2.2 -5" direction="alternate" dur="2000"
      repeat="indefinite"></a-animation>
  </a-box>
  <!-- ... -->
</a-scene>
```

### Animating on Events

```html
<a-box color="#FFF" width="4" height="10" depth="2"
       position="-10 2 -5" rotation="0 0 45" scale="2 0.5 3"
       src="#texture">
  <a-animation attribute="position" to="0 2.2 -5" direction="alternate" dur="2000"
    repeat="indefinite"></a-animation>
  <!-- These animations will start when the box is looked at. -->
  <a-animation attribute="scale" begin="mouseenter" dur="300" to="2.3 2.3 2.3"></a-animation>
  <a-animation attribute="scale" begin="mouseleave" dur="300" to="2 2 2"></a-animation>
  <a-animation attribute="rotation" begin="click" dur="2000" to="360 405 45"></a-animation>
</a-box>
```

## Adding Audio

Add an `<audio>` element under `<a-assets>`:

```html
<a-scene>
  <a-assets>
    <audio src="https://cdn.aframe.io/basic-guide/audio/backgroundnoise.wav" autoplay
      preload></audio>
  </a-assets>
  <!-- ... -->
</a-scene>
```

Or use `<a-sound>` inside the scene. This makes the sound get louder as we approach it and get softer as we distance from it.

```html
<a-scene>
  <!-- ... -->
  <a-sound src="https://cdn.aframe.io/basic-guide/audio/backgroundnoise.wav" autoplay="true"
    position="-3 1 -4"></a-sound>
  <!-- ... -->
</a-scene>
```

## Adding Text

* Text component:
    * https://aframe.io/docs/0.6.0/components/text.html
* Text Geometry:
    * https://github.com/ngokevin/kframe/tree/master/components/text-geometry/
* HTML Shader:
    * https://github.com/mayognaise/aframe-html-shader/
