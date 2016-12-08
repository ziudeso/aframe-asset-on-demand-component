## aframe-asset-on-demand-component

A Asset OnDemand component for [A-Frame](https://aframe.io). 

The A-Frame Asset OnDemand component does basically two things:

1. Load Asset on the fly when it is needed. Instead of declaring all the assets of the Scene in the <a-assets>-Section, you an use the Asset OnDemand Component to load and attach Assets during runtime when needed. This is especially beneficial if you have a lot of large assets (e.g. 360-Panoramas) but you only display one at a time. So it doesn't make sense to constantly have them all in memory or wait a long time to load all of them in the beginning.
2. Remove Asset from Memory when not needed anymore. If you just remove the Texture from an entity, it will not be removed from entirely from THREE.JS and still use precious memory (precious especially on mobile). The A-Frame Asset OnDemand makes sure, that the Asset is removed in a way that free's up memory.

Normally you would do something like this:

```html
<head>
  <title>My A-Frame Scene</title>
  <script src="https://aframe.io/releases/0.3.0/aframe.min.js"></script>
</head>

<body>
  <a-scene>
    <a-assets>
        <img id="large-image" src="images/large-image.jpg" />
    </a-assets>
    <a-box src="#large-image"></a-box>
  </a-scene>
</body>
```

This will load the Asset in the beginning and attach it to the box as a texture. It will stay there throughout the whole lifecycle of the scene.

With the Asset OnDemand Module you would it like this:

```html
<head>
  <title>My A-Frame Scene</title>
  <script src="https://aframe.io/releases/0.3.0/aframe.min.js"></script>
  <script src="https://rawgit.com/protyze/aframe-asset-on-demand-component/master/dist/aframe-asset-on-demand-component.min.js"></script>
</head>

<body>
  <a-scene>
    <a-assets>
    </a-assets>
    <a-box asset-on-demand="src: images/large-image.jpg"></a-box>
  </a-scene>
</body>
```

The are now different states during the lifecycle of the scene:

1. After Rendering: An img-Tag with an empty src-Attribute has been added to <a-assets>
2. After play (or any other configurable event): The resource is loaded within the img-Tag and attached as a Texture to the Box (via the material Component)
3. After pause (or any other configurable event): The Texture is removed from the box and the image is removed from <a-assets>
4. No more memory is used up
5. Steps 2./3. can now alternate

### API

| Property | Description | Default Value |
| -------- | ----------- | ------------- |
| src      | URL of the Asset that should be loaded | '' |
| type      | Type of the Asset (img, audio or video) | 'img' |
| attributes      | Attributes for the Asset-Entity that will be created on Demand (Format: attr1:value1,attr2:value2,...)  | '' |
| src      | URL of the Asset that should be loaded | '' |
| component      | Name of the A-Frame Component where the Asset will be assigned | 'material' |
| componentattr      | Attributes for the Component that will be added (Format: attr1:value1,attr2:value2,...) | '' |
| assetattr      | Attribute-Name of the Component, that will contain the Asset | 'src' |
| fallsback      | An Asset to use as default/fallback | '' |
| addevent      | Comma-seperated List of Events when the Asset should be attached | 'play' |
| removeevent      | Comma-seperated List of Events when the Asset should be detached | 'pause' |
| softmode      | If true only the texture will be removed on detached, not the asset | 'false' |


### Installation

#### Browser

Install and use by directly including the [browser files](dist):

```html
<head>
  <title>My A-Frame Scene</title>
  <script src="https://aframe.io/releases/0.3.0/aframe.min.js"></script>
  <script src="https://rawgit.com/protyze/aframe-asset-on-demand-component/master/dist/aframe-asset-on-demand-component.min.js"></script>
</head>

<body>
  <a-assets>
  </a-assets>
  <a-scene>
    <a-entity asset-on-demand="src: images/image.jpg"></a-entity>
  </a-scene>
</body>
```

#### npm

Install via npm:

```bash
npm install aframe-asset-on-demand-component
```

Then register and use.

```js
require('aframe');
require('aframe-asset-on-demand-component');
```
