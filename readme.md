Process rectangular shaders without webgl and obtain the reslut. Can be used for shaders unit testing, audio processing etc. See [glsl-hsl2rgb](https://github.com/Jam3/glsl-hsl2rgb) or [audio-shader](https://github.com/audio-lab/audio-shader) for practical examples.


[![npm install nogl-shader-output](https://nodei.co/npm/nogl-shader-output.png?mini=true)](https://npmjs.org/package/nogl-shader-output/)


```js
var ShaderOutput = require('nogl-shader-output')

//get a draw function for our test
var draw = ShaderOutput(`
    precision mediump float;
    uniform float green;
    void main() {
        gl_FragColor = vec4(0.0, green, 0.0, 1.0);
    }
`, {
    width: 1,
    height: 1
});

//returns the frag color as [R, G, B, A]
var color = draw()

//we could also set uniforms before rendering
var color2 = draw({ green: 0.5 })

//due to precision loss, you may want to use a fuzzy equality check
var epsilon = 1e-5;
var almostEqual = require('array-almost-equal')
almostEqual(color2, [0.0, 0.5, 0.0, 1.0], epsilon)
```

## API

#### `draw = ShaderOutput(source?, options?)`

Takes a shader object/source and an options object, and returns a `draw` function. Possible options:

- `shader` the shader, can be a source of fragment shader or an instance of gl-shader. Same as passing shader as the only argument.
- `width` the width of a drawing buffer, by default - 1
- `height` the height of a drawing buffer, by default - 1

The draw function has the following signature:

```js
var fragColor = draw(uniforms?)
```

Where `uniforms` is an optional map of uniform names to values (such as `[x, y]` array for vec2), applied before rendering.

The return value is the gl_FragColor RGBA of the canvas, in floats, such as `[0.5, 1.0, 0.25, 1.0]`.

## Related

* [gl-shader-output](http://npmjs.org/package/gl-shader-output) — a webgl browser version of fragment shader processor.
* [audio-shader](https://github.com/audio-lab/audio-shader) — an example case of application of nogl-shader-output for processing audio.