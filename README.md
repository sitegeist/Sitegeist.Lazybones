# Sitegeist.Lazybones
### Responsive Images for Neos - with Atomic.Fusion & Monocle in mind

This package implements Lazy Loading for Responsive Images from the [Sitegeist.Kaleidoscope](https://github.com/sitegeist/Sitegeist.Kaleidoscope) package.

This is implemented by rendering the `src`, `srcset` and `sizes` attributes as `data-src`, `data-srcset`
and `data-sizes`. The img- or pPicture-tag is then marked with the class `lazyload` to
be replaced via js once the image comes into view. If you like you can render a very lowres
src-attribute or leave the src attribute empty (default).

### Authors & Sponsors

* Martin Ficzel - ficzel@sitegeist.de

*The development and the public-releases of this package is generously sponsored
by our employer http://www.sitegeist.de.*

## Installation

Sitegeist.Lazybones is available via packagist run `composer require sitegeist/lazybones`.
We use semantic-versioning so every breaking change will increase the major-version number.

## Usage

### LazyloadJS

To enable lazy loading you have to integrate a script that will detect the `lazyload` class
and convert `data-src`, `data-srcset` and `data-sizes` to the correct attributes once they come
into view.

*This package includes the `lazysizes` library from https://github.com/aFarkas/lazysizes
but it is probably better to include the js into your own fe-build.*

To include the bundled js you can use the following snippet:

```
lazyloadJS = Neos.Fusion:Tag {
    tagName = 'script'
    attributes.async = true
    attributes.src = Neos.Fusion:ResourceUri {
        path = 'resource://Sitegeist.Lazybones/Public/JavaScript/lazysizes.min.js'
    }
}
```

### `Sitegeist.Lazybones:Image`

Render an img-tag with optional srcset based on sizes or resolutions.

This prototype is a drop in replacement for `Sitegeist.Kaleidoscope:Image` with
optional props to control the lazy loading.

See: https://github.com/sitegeist/Sitegeist.Kaleidoscope#sitegeistkaleidoscopeimage

Props:
- `lazy`: Enable lazy loading (boolean, defaults to `Sitegeist.Lazybones:Lazy.Enabled`)
- `lazyClass`: The class to attach to the img-tags (string, defaults to `Sitegeist.Lazybones:Lazy.ClassName`)
- `lazyWidth`: The width of the thumbnail-src that is loaded first  (string, defaults to `Sitegeist.Lazybones:Lazy.Width`)

Props from `Sitegeist.Kaleidoscope:Image`:
- `imageSource`: the imageSource to render
- `srcset`: media descriptors like '1.5x' or '600w' of the default image (string or array)
- `sizes`: sizes attribute of the default image (string or array)
- `format`: (optional) the image-format like `webp` or `png`, will be applied to the `imageSource`
- `width`: (optional) the base width, will be applied to the `imageSource`
- `height`: (optional) the base height, will be applied to the `imageSource`
- `alt`: alt-attribute for the img tag
- `title`: title attribute for the img tag
- `class`: class attribute for the img tag
- `renderDimensionAttributes`: render width/height attributes if data is available from the `imageSource` (boolean) 

### `Sitegeist.Lazybones:Picture`

Render a picture-tag with various sources.

This prototype is a drop in replacement for `Sitegeist.Kaleidoscope:Picture` with
optional props to control the lazy loading.

See: https://github.com/sitegeist/Sitegeist.Kaleidoscope#sitegeistkaleidoscopepicture

Props:
- `lazy`: Enable lazy loading (boolean, defaults to `Sitegeist.Lazybones:Lazy.Enabled`)
- `lazyClass`: The class to attach to the img-tags (string, defaults to `Sitegeist.Lazybones:Lazy.ClassName`)
- `lazyWidth`: The width of the thumbnail-src that is loaded first  (string, defaults to `Sitegeist.Lazybones:Lazy.Width`)

Props from `Sitegeist.Kaleidoscope:Picture`:
- `imageSource`: the imageSource to render
- `sources`: an array of source definitions that supports the following keys
   - `media`: the media query of this source
   - `type`: the type of this source
   - `imageSource`: alternate image-source for art direction purpose
   - `srcset`: media descriptors like '1.5x' or '600w' (string or array)
   - `sizes`: sizes attribute (string or array)
   - `width`: (optional) the base width, will be applied to the `imageSource`
   - `height`: (optional) the base height, will be applied to the `imageSource`
   - `format`: (optional) the image-format for the source like `webp` or `png`, is applied to `imageSource` and `type` 
- `srcset`: media descriptors like '1.5x' or '600w' of the default image (string or array)
- `sizes`: sizes attribute of the default image (string or array)
- `formats`: (optional) image formats that will be rendered as sources of separate type (string or array)
- `width`: (optional) the base width, will be applied to the `imageSource`
- `height`: (optional) the base height, will be applied to the `imageSource`
- `alt`: alt-attribute for the picture tag
- `title`: title attribute for the picture tag
- `class`: class attribute for the picture tag
- `renderDimensionAttributes`: render dimension attributes (width/height) for the img-tag when the data is available from the imageSource

### `Sitegeist.Lazybones:Source`

Render a source-tag with optional srcset based on sizes or resolutions.

This prototype is a drop in replacement for `Sitegeist.Kaleidoscope:Source` with
optional props to control the lazy loading.

Props:
- `lazy`: Enable lazy loading (boolean, defaults to `Sitegeist.Lazybones:Lazy.Enabled`)

Props from `Sitegeist.Kaleidoscope:Source`:
- `imageSource`: the imageSource to render
- `srcset`: media descriptors like '1.5x' or '600w' of the default image (string or array)
- `sizes`: sizes attribute of the default image (string or array)
- `media`: the media query of this source
- `type`: the type of this source
- `width`: (optional) the base width, will be applied to the `imageSource`
- `height`: (optional) the base height, will be applied to the `imageSource`
- `format`: the image-format for the source like `webp` or `png`, is applied to `imageSource` and `type`

### `Sitegeist.Lazybones:Lazy.Enabled`

Boolean value prototype with default value `true` that defines whether lazyness is enabled or not.
Override the `value` of this prototype globally or for specific parts of your fusion. 

### `Sitegeist.Lazybones:Lazy.ClassName`

String value prototype with default value `lazyload` to define the class that marks lazyloaded images.
Override the `value` of this prototype globally or for specific parts of your fusion. 

### `Sitegeist.Lazybones:Lazy.Width`

Integer value prototype with default value `null` to define the size of lowres images that are loaded as 
placeholders. Override zhe `value` of this prototype globally or for specific parts of your fusion. 

## Dynamically enable/disable the lazy rendering

To optimize the intial load time lazy loading should be disabled for the first contents but be enabled for others. This can be implemented by enabeling the `lazy` in the `ContentCase` prototype depending on wether or not the curent `node` is the first content in the main collection.

```
content = Neos.Neos:ContentCollection {
    nodePath = 'main'
    content.iterationName = 'mainContentIterator'

    // enable lazynes for all but first content 
    prototype(Sitegeist.Lazybones:Lazy.Enabled) {
        value = ${!mainContentIterator.isFirst}
    }
    
    // preload 150px variants of the images 
    prototype(Sitegeist.Lazybones:Lazy.Width) {
        value = 150
    }
}
```

## Contribution

We will gladly accept contributions. Please send us pull requests.
