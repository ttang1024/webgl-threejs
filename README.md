# webgl-learn

**Read this in other languages: [English](README.md) | [简体中文](README.zh-CN.md)**

Divided into two parts of learning: webgl and three.js

Reference materials:  
[WebGL Tutorial](https://webglfundamentals.org/webgl/lessons/)  
[Three.js Tutorial](http://www.webgl3d.cn/Three.js/)

The directory `webgl-demo` contains demo examples for learning WebGL chapters  
The directory `threejs-demo` contains demo examples for learning Three.js chapters

**The directory `threejs-playgame` is a comprehensive Three.js demo for learning Three.js, similar to the introductory guide in games, with text-to-speech for user chat and corresponding actions**

![demo](images/demo.gif)

```text
cd threejs-playgame
npm i
npm start
```

There is a face recognition login function based on face-api.js, and users enter the game after logging in.  
Path of the face recognition comparison image: threejs-playgame/src/resources/images/p1.png, which can be replaced with your own photo.  
In addition, you can modify the recognition accuracy data: `matchedDistance` in threejs-playgame/src/container/Login/face-detection.js.

```js
this.options = Object.assign(
	{
		matchedScore: 0.7,
		matchedDistance: 0.5,
	},
	options,
)
```

- [webgl-learn](#webgl-learn)
    - [webgl](#webgl)
        - [1. Working Principle](#1-working-principle)
        - [2. Basic Concepts](#2-basic-concepts)
            - [2.1. Vertex Shader](#21-vertex-shader)
                - [2.1.1. Attributes](#211-attributes)
                - [2.1.2. Uniforms](#212-uniforms)
            - [2.2. Fragment Shader](#22-fragment-shader)
                - [2.2.1. Uniforms (in Fragment Shader)](#221-uniforms-in-fragment-shader)
                - [2.2.2. Textures (in Fragment Shader)](#222-textures-in-fragment-shader)
                - [2.2.3. Varyings](#223-varyings)
            - [2.3. GLSL](#23-glsl)
        - [3. Common APIs](#3-common-apis)
        - [4. Affine Transformation](#4-affine-transformation)
            - [4.1. Translation, Rotation and Scaling of Vectors](#41-translation-rotation-and-scaling-of-vectors)
            - [4.2. Scaling Transformation](#42-scaling-transformation)
            - [4.3. Application of Affine Transformation: Implementing Particle Animation](#43-application-of-affine-transformation-implementing-particle-animation)
                - [4.3.1. Creating Triangles](#431-creating-triangles)
                - [4.3.2. Setting Uniform Variables](#432-setting-uniform-variables)
                - [4.4.3. Implementing Animation with requestAnimationFrame](#443-implementing-animation-with-requestanimationframe)
        - [5. Drawing Repeating Patterns](#5-drawing-repeating-patterns)
            - [5.1. Drawing Repeating Patterns with background-image](#51-drawing-repeating-patterns-with-background-image)
            - [5.2. Drawing Repeating Patterns with Shader](#52-drawing-repeating-patterns-with-shader)
        - [6. Implementing Pixel Animation with Shaders](#6-implementing-pixel-animation-with-shaders)
            - [6.1. Implementing Fixed Frame Animation with Shaders](#61-implementing-fixed-frame-animation-with-shaders)
            - [6.2. Implementing Non-Fixed Frame Animation with Shaders](#62-implementing-non-fixed-frame-animation-with-shaders)
                - [6.2.1. Implementing Non-Fixed Frame Animation with Vertex Shader](#621-implementing-non-fixed-frame-animation-with-vertex-shader)
                - [6.2.2. Implementing Non-Fixed Frame Animation with Fragment Shader](#622-implementing-non-fixed-frame-animation-with-fragment-shader)
            - [6.3. How to Implement Easing Functions and Non-Linear Interpolation in Shaders](#63-how-to-implement-easing-functions-and-non-linear-interpolation-in-shaders)
        - [7. Drawing 3D Cubes with WebGL](#7-drawing-3d-cubes-with-webgl)
        - [8. Camera and View Matrix](#8-camera-and-view-matrix)
    - [three.js](#threejs)
        - [1. Program Structure](#1-program-structure)
            - [1.1. Material](#11-material)
                - [Material Properties](#material-properties)
                - [Adding Specular Highlights](#adding-specular-highlights)
            - [1.2. Light](#12-light)
                - [Common Light Types](#common-light-types)
            - [1.3. Camera](#13-camera)
            - [1.4. Mouse Interaction with 3D Scene](#14-mouse-interaction-with-3d-scene)
        - [2. Geometry Vertices](#2-geometry-vertices)
            - [Geometry APIs](#geometry-apis)
            - [2.1. Vertex Position Data Parsing \& Rendering](#21-vertex-position-data-parsing--rendering)
                - [Point Cloud (`Points`)](#point-cloud-points)
                - [Line (`Line`)](#line-line)
            - [2.2. Vertex Color Data Interpolation](#22-vertex-color-data-interpolation)
                - [Assign a Color to Each Vertex](#assign-a-color-to-each-vertex)
            - [2.3. Vertex Normal Data for Lighting Calculations](#23-vertex-normal-data-for-lighting-calculations)
            - [2.4. Vertex Indices for Reusing Vertex Data](#24-vertex-indices-for-reusing-vertex-data)
                - [`index` Attribute](#index-attribute)
            - [2.5. `Face3` for Defining Triangular Faces (Geometry)](#25-face3-for-defining-triangular-faces-geometry)
                - [Setting Triangle Normals](#setting-triangle-normals)
                - [Setting Triangle Colors](#setting-triangle-colors)
            - [2.6. Accessing Geometry Data](#26-accessing-geometry-data)
            - [2.7. Geometry Transformations (Scale/Rotate/Translate)](#27-geometry-transformations-scalerotatetranslate)
        - [3. Materials](#3-materials)
            - [3.1. `PointsMaterial` (Point Material)](#31-pointsmaterial-point-material)
            - [3.2. Line Materials](#32-line-materials)
            - [3.3. Mesh Materials](#33-mesh-materials)
            - [3.4. Material-Model Compatibility](#34-material-model-compatibility)
            - [3.5. `.side` Property](#35-side-property)
            - [3.6. Transparency (`opacity`)](#36-transparency-opacity)
        - [4. Points, Lines, and Meshes](#4-points-lines-and-meshes)
            - [4.1. Points (`Points`)](#41-points-points)
            - [4.2. Lines (`Line`)](#42-lines-line)
            - [4.3. Meshes (`Mesh`)](#43-meshes-mesh)
            - [4.4. Cloning (`clone()`) and Copying (`copy()`)](#44-cloning-clone-and-copying-copy)
        - [2.5. Lights](#25-lights)
            - [2.5.1. AmbientLight](#251-ambientlight)
            - [2.5.2. PointLight](#252-pointlight)
            - [2.5.3. DirectionalLight](#253-directionallight)
            - [2.5.4. SpotLight](#254-spotlight)
            - [2.5.5. Lighting Calculation Algorithm](#255-lighting-calculation-algorithm)
            - [2.5.6. Directional Light Shadow Calculation](#256-directional-light-shadow-calculation)
            - [2.5.7. SpotLight Shadow Calculation](#257-spotlight-shadow-calculation)
        - [2.6. Hierarchical Models (Tree Structure)](#26-hierarchical-models-tree-structure)
            - [2.6.1. Group Object \& Hierarchical Models](#261-group-object--hierarchical-models)
            - [2.6.2. Naming, Finding, and Traversing Nodes](#262-naming-finding-and-traversing-nodes)
            - [2.6.3. Coordinates](#263-coordinates)
        - [2.7. Geometry, Curves, and 3D Models](#27-geometry-curves-and-3d-models)
            - [2.7.1. Geometry](#271-geometry)
            - [2.7.2. Curves](#272-curves)
            - [2.7.3. Spline \& Bezier Curves](#273-spline--bezier-curves)
            - [2.7.4. Composite Curves (`CurvePath`)](#274-composite-curves-curvepath)
            - [2.7.5. TubeGeometry (Extrude Curve into Tube)](#275-tubegeometry-extrude-curve-into-tube)
            - [2.7.6. LatheGeometry (Revolution Surface)](#276-lathegeometry-revolution-surface)
            - [2.7.7. Shape \& ShapeGeometry (Filled Contours)](#277-shape--shapegeometry-filled-contours)
        - [2.8. Textures](#28-textures)
            - [2.8.1. Create a Texture](#281-create-a-texture)
            - [2.8.2. UV Coordinates](#282-uv-coordinates)
            - [2.8.3. Multi-Materials \& Material Indices](#283-multi-materials--material-indices)
            - [2.8.4. Texture Repeat/Offset/Rotation](#284-texture-repeatoffsetrotation)
            - [2.8.5. DataTexture (Dynamic Textures)](#285-datatexture-dynamic-textures)
            - [2.8.6. Advanced Textures](#286-advanced-textures)
        - [2.9. Cameras](#29-cameras)
            - [2.9.1. Orthographic vs. Perspective Cameras](#291-orthographic-vs-perspective-cameras)
                - [OrthographicCamera](#orthographiccamera)
                - [PerspectiveCamera](#perspectivecamera)
        - [2.10. TextGeometry (3D Text)](#210-textgeometry-3d-text)
        - [2.11. Animation System](#211-animation-system)
            - [KeyframeTrack](#keyframetrack)
            - [AnimationClip](#animationclip)
            - [AnimationMixer \& AnimationAction](#animationmixer--animationaction)
        - [2.12. Skeletal \& Morph Target Animation](#212-skeletal--morph-target-animation)
            - [Skeletal Animation (SkinnedMesh)](#skeletal-animation-skinnedmesh)
        - [2.13. Audio](#213-audio)
        - [2.14. Model Loading](#214-model-loading)
            - [Export Three.js Models](#export-threejs-models)
            - [Load Custom Models](#load-custom-models)
        - [2.15. Performance Analysis \& Optimization](#215-performance-analysis--optimization)
            - [2.15.1. Stats.js (Performance Monitor)](#2151-statsjs-performance-monitor)
            - [2.15.2. Key Optimization Tips](#2152-key-optimization-tips)

## webgl

### 1. Working Principle

WebGL runs on the computer's GPU. Therefore, you need to use code that can run on the GPU. Such code needs to provide pairs of functions. Each pair consists of a vertex shader and a fragment shader, and uses a strongly typed language similar to C or C++ called GLSL (GL Shading Language). Each pair combined is called a program (shader program).

The role of the vertex shader is to calculate the position of vertices. Based on the calculated series of vertex positions, WebGL can rasterize primitives such as points, lines, and triangles. When rasterizing these primitives, the fragment shader function is used. The role of the fragment shader is to calculate the color value of each pixel in the currently drawn primitive.

WebGL's work on the GPU is basically divided into two parts: the first part is to convert vertices (or data streams) to clip space coordinates, and the second part is to draw pixels based on the results of the first part.

```js
var primitiveType = gl.TRIANGLES
var offset = 0
var count = 9
gl.drawArrays(primitiveType, offset, count)
```

9 means "process 9 vertices", so 9 vertices will be transformed.

![Working Principle](images/vertex-shader-anim.gif)

A vertex shader is a function you write in GLSL that is called once per vertex. After performing some mathematical operations in this function, you set a special variable `gl_Position`, which is the coordinate value of the vertex transformed into clip space. The GPU receives this value and saves it.

The WebGL drawing process includes the following three steps:

1. Obtain vertex coordinates
2. Primitive assembly (i.e., drawing triangles one by one)
3. Rasterization (generating fragments, i.e., pixels one by one)

![WebGL Drawing Process](images/webgl-1.png)

### 2. Basic Concepts

WebGL requires two shaders for each draw call: a vertex shader and a fragment shader, each of which is a function. A vertex shader and a fragment shader are linked together into a shader program (or just called a program). A typical WebGL application will have multiple shader programs.

#### 2.1. Vertex Shader

The job of a vertex shader is to generate clip space coordinate values.

```js
void main() {
   gl_Position = doMathToMakeClipspaceCoordinates;
}
```

The (vertex) shader is called once per vertex, and each call needs to set a special global variable `gl_Position`, whose value is the clip space coordinate value.

The data required by the vertex shader can be obtained in the following three ways:

1. Attributes (data obtained from buffers)
2. Uniforms (global variables with consistent values for all vertices in a single draw call)
3. Textures (data obtained from pixels or texels)

##### 2.1.1. Attributes

Create a buffer:

```js
var buf = gl.createBuffer()
```

Store data in the buffer:

```js
gl.bindBuffer(gl.ARRAY_BUFFER, buf)
gl.bufferData(gl.ARRAY_BUFFER, someData, gl.STATIC_DRAW)
```

During initialization, find the address of the attribute in the created (shader) program:

```js
var positionLoc = gl.getAttribLocation(someShaderProgram, 'a_position')
```

During rendering, tell WebGL how to get data from the buffer and pass it to the attribute:

```js
// Enable getting data from the buffer
gl.enableVertexAttribArray(positionLoc)

var numComponents = 3 // (x, y, z)
var type = gl.FLOAT // 32-bit floating-point data
var normalize = false // Do not normalize
var offset = 0 // Start getting data from the beginning of the buffer
var stride = 0 // How many bytes of memory to skip to the next data
// 0 = Use the current number of units and unit length ( 3 * Float32Array.BYTES_PER_ELEMENT )

// Bind the current buffer range to gl.ARRAY_BUFFER, become the generic vertex attribute of the current vertex buffer object and specify its layout (offset in the buffer object).
gl.vertexAttribPointer(positionLoc, numComponents, type, false, stride, offset)
```

Pass data directly to `gl_Position` without any operations:

```js
attribute vec4 a_position;

void main() {
   gl_Position = a_position;
}
```

This works if the buffer stores clip space coordinates.

Attributes can use data types such as float, vec2, vec3, vec4, mat2, mat3, and mat4.

Vectors:  
vec{2,3,4} - float vectors of length 2, 3, 4  
bvec{2,3,4} - bool vectors of length 2, 3, 4  
ivec{2,3,4} - int vectors of length 2, 3, 4  
Matrices:  
mat2 - 2*2 floating-point matrix  
mat3 - 3*3 floating-point matrix  
mat4 - 4\*4 floating-point matrix

##### 2.1.2. Uniforms

Uniforms are variables passed to shaders with the same value throughout a single draw call. In the following simple example, a uniform variable is used to add an offset to the vertex shader:

```js
attribute vec4 a_position;
uniform vec4 u_offset;

void main() {
   gl_Position = a_position + u_offset;
}
```

To offset all vertices by a fixed value, first find the address of the uniform variable during initialization:

```js
var offsetLoc = gl.getUniformLocation(someProgram, 'u_offset')
```

Then set the uniform variable before drawing:

```js
gl.uniform4fv(offsetLoc, [1, 0, 0, 0]) // Offset half the screen width to the right
```

> Note that uniforms belong to a single shader program. If multiple shader programs have uniform variables with the same name, you need to find each uniform variable and set its own value.

Uniforms have many types, and there are corresponding setting methods for each type:

```js
gl.uniform1f (floatUniformLoc, v);                 // float
gl.uniform1fv(floatUniformLoc, [v]);               // float or float array
gl.uniform2f (vec2UniformLoc,  v0, v1);            // vec2
gl.uniform2fv(vec2UniformLoc,  [v0, v1]);          // vec2 or vec2 array
gl.uniform3f (vec3UniformLoc,  v0, v1, v2);        // vec3
gl.uniform3fv(vec3UniformLoc,  [v0, v1, v2]);      // vec3 or vec3 array
gl.uniform4f (vec4UniformLoc,  v0, v1, v2, v4);    // vec4
gl.uniform4fv(vec4UniformLoc,  [v0, v1, v2, v4]);  // vec4 or vec4 array

gl.uniformMatrix2fv(mat2UniformLoc, false, [  4x element array ])  // mat2 or mat2 array
gl.uniformMatrix3fv(mat3UniformLoc, false, [  9x element array ])  // mat3 or mat3 array
gl.uniformMatrix4fv(mat4UniformLoc, false, [ 16x element array ])  // mat4 or mat4 array

gl.uniform1i (intUniformLoc,   v);                 // int
gl.uniform1iv(intUniformLoc, [v]);                 // int or int array
gl.uniform2i (ivec2UniformLoc, v0, v1);            // ivec2
gl.uniform2iv(ivec2UniformLoc, [v0, v1]);          // ivec2 or ivec2 array
gl.uniform3i (ivec3UniformLoc, v0, v1, v2);        // ivec3
gl.uniform3iv(ivec3UniformLoc, [v0, v1, v2]);      // ivec3 or ivec3 array
gl.uniform4i (ivec4UniformLoc, v0, v1, v2, v4);    // ivec4
gl.uniform4iv(ivec4UniformLoc, [v0, v1, v2, v4]);  // ivec4 or ivec4 array

gl.uniform1i (sampler2DUniformLoc,   v);           // sampler2D (textures)
gl.uniform1iv(sampler2DUniformLoc, [v]);           // sampler2D or sampler2D array

gl.uniform1i (samplerCubeUniformLoc,   v);         // samplerCube (textures)
gl.uniform1iv(samplerCubeUniformLoc, [v]);         // samplerCube or samplerCube array
```

An array can set all uniform variables at once, for example:

```js
// In the shader
uniform vec2 u_someVec2[3];

// During JavaScript initialization
var someVec2Loc = gl.getUniformLocation(someProgram, "u_someVec2");

// During rendering
gl.uniform2fv(someVec2Loc, [1, 2, 3, 4, 5, 6]);  // Set the array u_someVec2
```

To set a single value in the array individually, you need to find the address of that value individually:

```js
// During JavaScript initialization
var someVec2Element0Loc = gl.getUniformLocation(someProgram, 'u_someVec2[0]')
var someVec2Element1Loc = gl.getUniformLocation(someProgram, 'u_someVec2[1]')
var someVec2Element2Loc = gl.getUniformLocation(someProgram, 'u_someVec2[2]')

// During rendering
gl.uniform2fv(someVec2Element0Loc, [1, 2]) // set element 0
gl.uniform2fv(someVec2Element1Loc, [3, 4]) // set element 1
gl.uniform2fv(someVec2Element2Loc, [5, 6]) // set element 2
```

If you create a struct, you need to find the address of each element:

```js
struct SomeStruct {
  bool active;
  vec2 someVec2;
};
uniform SomeStruct u_someThing;

var someThingActiveLoc = gl.getUniformLocation(someProgram, "u_someThing.active");
var someThingSomeVec2Loc = gl.getUniformLocation(someProgram, "u_someThing.someVec2");
```

#### 2.2. Fragment Shader

The job of a fragment shader is to provide color values for the currently rasterized pixels, usually in the following form:

```js
precision mediump float;

void main() {
   gl_FragColor = doMathToMakeAColor;
}
```

The fragment shader is called once per pixel, and each call needs to get color information from the special global variable `gl_FragColor` that you set.

The data required by the fragment shader can be obtained in the following three ways:

Uniforms  
Textures  
Varyings

##### 2.2.1. Uniforms (in Fragment Shader)

Same as Uniforms.

##### 2.2.2. Textures (in Fragment Shader)

To get texture information in the shader, you can first create a uniform variable of type sampler2D, then use the GLSL function `texture2D` to extract information from the texture:

```js
precision mediump float;

uniform sampler2D u_texture;

void main() {
   vec2 texcoord = vec2(0.5, 0.5);  // Get the value at the center of the texture
   gl_FragColor = texture2D(u_texture, texcoord);
}
```

The data obtained from the texture depends on many settings. At a minimum, you need to create and fill the texture with data, for example:

```js
var tex = gl.createTexture()
gl.bindTexture(gl.TEXTURE_2D, tex)
var level = 0
var width = 2
var height = 1
var data = new Uint8Array([
	255,
	0,
	0,
	255, // A red pixel
	0,
	255,
	0,
	255, // A green pixel
])
gl.texImage2D(gl.TEXTURE_2D, level, gl.RGBA, width, height, 0, gl.RGBA, gl.UNSIGNED_BYTE, data)

// Find the address of the uniform variable during initialization
var someSamplerLoc = gl.getUniformLocation(someProgram, 'u_texture')

// During rendering, WebGL requires the texture to be bound to a texture unit
var unit = 5 // Pick a texture unit
gl.activeTexture(gl.TEXTURE0 + unit)
gl.bindTexture(gl.TEXTURE_2D, tex)

// Tell the shader which texture unit the texture you want to use is on
gl.uniform1i(someSamplerLoc, unit)
```

##### 2.2.3. Varyings

Varyings are a way for vertex shaders to pass values to fragment shaders.

To use varyings, you need to define varyings with the same name in both shaders. The value set for the varying in the vertex shader will be interpolated as a reference value and passed to the varying in the fragment shader when drawing pixels.

```js
attribute vec4 a_position;

uniform vec4 u_offset;

varying vec4 v_positionWithOffset;

void main() {
  gl_Position = a_position + u_offset;
  v_positionWithOffset = a_position + u_offset;
}
```

#### 2.3. GLSL

GLSL stands for Graphics Library Shader Language, which is the language used by shaders.

Its purpose is to provide common computing functions for rasterized graphics. Therefore, it has built-in data types such as vec2, vec3, and vec4 representing two, three, and four values respectively, and similarly mat2, mat3, and mat4 representing 2x2, 3x3, and 4x4 matrices respectively.

```js
// Multiplication of constants and vectors.
vec4 a = vec4(1, 2, 3, 4);
vec4 b = a * 2.0;// b is now vec4(2, 4, 6, 8);

// Perform matrix multiplication and vector-matrix multiplication
mat4 a = ???
mat4 b = ???
mat4 c = a * b;

vec4 v = ???
vec4 y = c * v;
```

It provides multiple component selectors for vector data, for example, for vec4:

```js
vec4 v;

// v.x, v.s, v.r, and v[0] represent the same component.
// v.y, v.t, v.g, and v[1] represent the same component.
// v.z, v.p, v.b, and v[2] represent the same component.
// v.w, v.q, v.a, and v[3] represent the same component.


// Supports vector swizzling, meaning you can swap or repeat components.
// These are the same
v.yyyy

vec4(v.y, v.y, v.y, v.y)

// These are the same
vec4 m = mix(v1, v2, f);
vec4 m = vec4(
  mix(v1.x, v2.x, f),
  mix(v1.y, v2.y, f),
  mix(v1.z, v2.z, f),
  mix(v1.w, v2.w, f));
```

### 3. Common APIs

1. gl.createShader — Create a shader object

```js
const vertexShader = gl.createShader(gl.VERTEX_SHADER)
```

2. gl.shaderSource — Set the GLSL program code for a WebGLShader (vertex shader and fragment shader)

```js
const vertex = `
      attribute vec2 position;
      varying vec3 color;

      void main() {
        gl_PointSize = 1.0;
        color = vec3(0.5 + position * 0.5, 0.0);
        gl_Position = vec4(position * 0.5, 1.0, 1.0);
      }
    `

const fragment = `
      precision mediump float;
      varying vec3 color;

      void main()
      {
        gl_FragColor = vec4(color, 1.0);
      }    
    `

const vertexShader = gl.createShader(gl.VERTEX_SHADER)
gl.shaderSource(vertexShader, vertex)
gl.compileShader(vertexShader)

const fragmentShader = gl.createShader(gl.FRAGMENT_SHADER)
gl.shaderSource(fragmentShader, fragment)
gl.compileShader(fragmentShader)
```

3. gl.compileShader — Compile a GLSL shader into binary data so that it can be used by a WebGLProgram object.

See the code block above

4. gl.createProgram — Create and initialize a WebGLProgram object.

```js
const program = gl.createProgram()
```

5. gl.attachShader — Add a fragment or vertex shader to a WebGLProgram.

```js
gl.attachShader(program, vertexShader)
gl.attachShader(program, fragmentShader)
```

6. gl.linkProgram — Link the given WebGLProgram, thereby completing the process of preparing GPU code for the program's fragment and vertex shaders.

```js
gl.linkProgram(program)
```

7. gl.useProgram — Add the defined WebGLProgram object to the current rendering state.

```js
gl.useProgram(program)
```

8. gl.createBuffer — Create and initialize a WebGLBuffer object for storing vertex data or shader data

```js
const bufferId = gl.createBuffer()
const bufferId = gl.createBuffer()
```

9. gl.bindBuffer — Set the buffer as the currently used buffer

```js
gl.bindBuffer(gl.ARRAY_BUFFER, bufferId)
```

10. gl.bufferData — Copy data to the buffer

```js
const points = new Float32Array([-1, -1, 0, 1, 1, -1])
gl.bufferData(gl.ARRAY_BUFFER, points, gl.STATIC_DRAW)
```

11. gl.getAttribLocation — Return the index position of an attribute in a given WebGLProgram object.

```js
const vPosition = gl.getAttribLocation(program, 'position')
gl.vertexAttribPointer(vPosition, 2, gl.FLOAT, false, 0, 0)
gl.enableVertexAttribArray(vPosition)
```

12. gl.vertexAttribPointer — Tell the graphics card to read vertex data from the currently bound buffer (the buffer specified by bindBuffer())

```js
gl.vertexAttribPointer(index, size, type, normalized, stride, offset)
// index Specifies the index of the vertex attribute to be modified.

// size Specifies the number of components per vertex attribute, must be 1, 2, 3, or 4...

// type Specifies the data type of each element in the array, possible values:
// gl.BYTE: signed 8-bit integer, with values in [-128, 127]
// gl.SHORT: signed 16-bit integer, with values in [-32768, 32767]
// gl.UNSIGNED_BYTE: unsigned 8-bit integer, with values in [0, 255]
// gl.UNSIGNED_SHORT: unsigned 16-bit integer, with values in [0, 65535]
// gl.FLOAT: 32-bit IEEE floating point number
// For WebGL2 version, the following value can also be used: gl.HALF_FLOAT: 16-bit IEEE floating point number

// normalized Whether integer values should be normalized to a specific range when converted to floating-point numbers.
// For types gl.BYTE and gl.SHORT, if true, values are normalized to [-1, 1]
// For types gl.UNSIGNED_BYTE and gl.UNSIGNED_SHORT, if true, values are normalized to [0, 1]
// This parameter is invalid for types gl.FLOAT and gl.HALF_FLOAT

// stride A GLsizei, specifying the offset (i.e., the length of a row in the array) in bytes between the start of consecutive vertex attributes. Cannot be greater than 255. If stride is 0, the attribute is assumed to be tightly packed, i.e., no interleaved attributes, each attribute is in a separate block, and the attribute of the next vertex immediately follows the current vertex.

// offset GLintptr specifies the byte offset of the first part in the vertex attribute array. Must be a multiple of the byte length of the type.
```

13. gl.enableVertexAttribArray — Enable the generic vertex attribute array at the specified index in the attribute array list. The vertex attribute array can be disabled via the disableVertexAttribArray() method.

See the code block above

14. gl.clear — Clear the specified buffer to a preset value.

```js
gl.clear(gl.COLOR_BUFFER_BIT)
```

15. gl.drawArrays — Render the original data in the array.

```js
gl.drawArrays(gl.TRIANGLES, 0, points.length / 2)
```

### 4. Affine Transformation

Affine transformation is simply "linear transformation + translation".

1. A line segment before affine transformation remains a line segment after affine transformation.

2. Applying the same affine transformation to two line segments a and b, the ratio of the lengths of the segments before and after the transformation remains unchanged.

#### 4.1. Translation, Rotation and Scaling of Vectors

Common forms of affine transformation include translation, rotation, scaling, and their combinations. Among them, translation transformation is the simplest form of affine transformation. If we want to translate the vector P(x0, y0) along the vector Q(x1, y1), we just need to add P and Q.

x = x0 + x1  
y = y0 + y1

Rotation transformation

```js
class Vector2D {
  ...
  rotate(rad) {
    const c = Math.cos(rad),
      s = Math.sin(rad);
    const [x, y] = this;

    this.x = x * c + y * -s;
    this.y = x * s + y * c;

    return this;
  }
}
```

![p1](images/p1.jpg)

Assume the length of vector P is r and the angle is α. Now we want to rotate it clockwise by θ angle. At this time, the parametric equation of the new vector P’ is:

![p2](images/p2.jpeg)

Since rcosα and rsinα are the original coordinates x0 and y0 of vector P, we can substitute the coordinates into the above formula to get the following formula:

![p3](images/p3.jpeg)

We then write it in matrix form to get a rotation matrix:

![p4](images/p4.jpeg)

#### 4.2. Scaling Transformation

Directly multiply the vector by a scalar (a scalar has only magnitude, no direction).

x = sx _ x0  
y = sy _ y0

#### 4.3. Application of Affine Transformation: Implementing Particle Animation

Generating many small randomly moving graphics over a certain period of time, such animations usually aim to capture users' attention through visual impact.

The running effect of particle animation is to emit many triangles with different colors, sizes, and angles from a single point, and continuously change their positions to produce a visual effect like scattering flowers.

##### 4.3.1. Creating Triangles

Define the vertices of the triangle and send the data to the buffer:

```js
const position = new Float32Array([-1, -1, 0, 1, 1, -1])
const bufferId = gl.createBuffer()
gl.bindBuffer(gl.ARRAY_BUFFER, bufferId)
gl.bufferData(gl.ARRAY_BUFFER, position, gl.STATIC_DRAW)

const vPosition = gl.getAttribLocation(program, 'position')
gl.vertexAttribPointer(vPosition, 2, gl.FLOAT, false, 0, 0)
gl.enableVertexAttribArray(vPosition)
```

Create random triangle properties:

```js
function randomTriangles() {
	const u_color = [Math.random(), Math.random(), Math.random(), 1.0] // Random color
	const u_rotation = Math.random() * Math.PI // Initial rotation angle
	const u_scale = Math.random() * 0.05 + 0.03 // Initial size
	const u_time = 0
	const u_duration = 3.0 // Lasts for 3 seconds

	const rad = Math.random() * Math.PI * 2
	const u_dir = [Math.cos(rad), Math.sin(rad)] // Movement direction
	const startTime = performance.now()

	return { u_color, u_rotation, u_scale, u_time, u_duration, u_dir, startTime }
}
```

##### 4.3.2. Setting Uniform Variables

Variables declared with uniform are like constants in other languages. The values we assign to uniform variables cannot be changed during the execution of the shader. Moreover, the value of a variable is unique and does not change with vertices. Uniform variables can be used in both vertex shaders and fragment shaders.

```js
function setUniforms(gl, { u_color, u_rotation, u_scale, u_time, u_duration, u_dir }) {
	// gl.getUniformLocation gets the pointer to the uniform variable
	let loc = gl.getUniformLocation(program, 'u_color')
	// Pass data to the address of the uniform variable
	gl.uniform4fv(loc, u_color)

	loc = gl.getUniformLocation(program, 'u_rotation')
	gl.uniform1f(loc, u_rotation)

	loc = gl.getUniformLocation(program, 'u_scale')
	gl.uniform1f(loc, u_scale)

	loc = gl.getUniformLocation(program, 'u_time')
	gl.uniform1f(loc, u_time)

	loc = gl.getUniformLocation(program, 'u_duration')
	gl.uniform1f(loc, u_duration)

	loc = gl.getUniformLocation(program, 'u_dir')
	gl.uniform2fv(loc, u_dir)
}
```

GLSL code in the vertex shader:

```js

attribute vec2 position;

uniform float u_rotation;
uniform float u_time;
uniform float u_duration;
uniform float u_scale;
uniform vec2 u_dir;

varying float vP;

void main() {
  float p = min(1.0, u_time / u_duration);
  float rad = u_rotation + 3.14 * 10.0 * p;
  float scale = u_scale * p * (2.0 - p);
  vec2 offset = 2.0 * u_dir * p * p;
  mat3 translateMatrix = mat3(
    1.0, 0.0, 0.0,
    0.0, 1.0, 0.0,
    offset.x, offset.y, 1.0
  );
  mat3 rotateMatrix = mat3(
    cos(rad), sin(rad), 0.0,
    -sin(rad), cos(rad), 0.0,
    0.0, 0.0, 1.0
  );
  mat3 scaleMatrix = mat3(
    scale, 0.0, 0.0,
    0.0, scale, 0.0,
    0.0, 0.0, 1.0
  );
  gl_PointSize = 1.0;
  vec3 pos = translateMatrix * rotateMatrix * scaleMatrix * vec3(position, 1.0);
  gl_Position = vec4(pos, 1.0);
  vP = p;
}
```

Coloring in the fragment shader:

```js

 precision mediump float;
  uniform vec4 u_color;
  varying float vP;

  void main()
  {
    gl_FragColor.xyz = u_color.xyz;
    gl_FragColor.a = (1.0 - vP) * u_color.a;
  }
```

##### 4.4.3. Implementing Animation with requestAnimationFrame

```js
let triangles = []

function update() {
	for (let i = 0; i < 5 * Math.random(); i++) {
		triangles.push(randomTriangles())
	}
	gl.clear(gl.COLOR_BUFFER_BIT)
	// Reset u_time for each triangle
	triangles.forEach(triangle => {
		triangle.u_time = (performance.now() - triangle.startTime) / 1000
		setUniforms(gl, triangle)
		gl.drawArrays(gl.TRIANGLES, 0, position.length / 2)
	})
	// Remove triangles whose animation has ended
	triangles = triangles.filter(triangle => {
		return triangle.u_time <= triangle.u_duration
	})
	requestAnimationFrame(update)
}

requestAnimationFrame(update)
```

### 5. Drawing Repeating Patterns

#### 5.1. Drawing Repeating Patterns with background-image

```css
canvas {
	background-image:
		linear-gradient(to right, transparent 90%, #ccc 0),
		linear-gradient(to bottom, transparent 90%, #ccc 0);
	background-size:
		8px 8px,
		8px 8px;
}
```

Using the browser's own background-repeat mechanism, you can implement a grid background.

![Grid Image](images/p5.jpeg)

#### 5.2. Drawing Repeating Patterns with Shader

Taking advantage of the parallel computing characteristics of the GPU, use shaders to draw repeating patterns such as background grids.

```js

// Vertex shader:

attribute vec2 a_vertexPosition;
attribute vec2 uv;
varying vec2 vUv;


void main() {
  gl_PointSize = 1.0;
  vUv = uv;
  gl_Position = vec4(a_vertexPosition, 1, 1);


// Fragment shader:


#ifdef GL_ES
precision mediump float;
#endif
varying vec2 vUv;
uniform float rows;

void main() {
  vec2 st = fract(vUv * rows);
  float d1 = step(st.x, 0.9);
  float d2 = step(0.1, st.y);
  gl_FragColor.rgb = mix(vec3(0.8), vec3(1.0), d1 * d2);
  gl_FragColor.a = 1.0;
}

```

A basic library gl-renderer. gl-renderer provides some simple encapsulation on the basis of the underlying WebGL, so that we can focus on providing geometric data, setting variables, and writing Shaders, without being distracted by details such as creating buffers.

```js
// Step 1: Create a Renderer object
const canvas = document.querySelector('canvas')
const renderer = new GlRenderer(canvas)

// Step 2: Create and enable a WebGL program
const program = renderer.compileSync(fragment, vertex)
renderer.useProgram(program)

// Step 3: Set uniform variables
renderer.uniforms.rows = 64

// Step 4: Send vertex data to the buffer.

renderer.setMeshData([
	{
		positions: [
			[-1, -1],
			[-1, 1],
			[1, 1],
			[1, -1],
		],
		attributes: {
			uv: [
				[0, 0],
				[0, 1],
				[1, 1],
				[1, 0],
			],
		},
		cells: [
			[0, 1, 2],
			[2, 0, 3],
		],
	},
])
```

### 6. Implementing Pixel Animation with Shaders

#### 6.1. Implementing Fixed Frame Animation with Shaders

Implement fixed frame animation very simply by replacing texture coordinates in the fragment shader.

```js

#ifdef GL_ES
precision highp float;
#endif

varying vec2 vUv;
uniform sampler2D tMap;
uniform float fWidth;
uniform vec2 vFrames[3];
uniform int frameIndex;

void main() {
  vec2 uv = vUv;
  for (int i = 0; i < 3; i++) {
    uv.x = mix(vFrames[i].x, vFrames[i].y, vUv.x) / fWidth;
    if(float(i) == mod(float(frameIndex), 3.0)) break;
  }
  vec4 color = texture2D(tMap, uv);
  gl_FragColor = color;
}
```

The key part of implementing fixed frame animation with fragment shaders is the for loop in the main function. Since our animation has only 3 frames, we only need to loop up to 3 times.

We also need an important parameter, vFrames. It is the starting x and ending x coordinates of the image for each frame of animation. We use these two coordinates and vUv.x to calculate the interpolation, and finally divide by the total width of the image fWidth to get the corresponding texture x coordinate. After replacing the texture coordinates, we can implement a flying bird.

#### 6.2. Implementing Non-Fixed Frame Animation with Shaders

##### 6.2.1. Implementing Non-Fixed Frame Animation with Vertex Shader

First draw a red square, then implement rotation using a 3D homogeneous matrix. Specifically, perform matrix operations on the vertex coordinates.

```js

attribute vec2 a_vertexPosition;
attribute vec2 uv;

varying vec2 vUv;
uniform float rotation;

void main() {
  gl_PointSize = 1.0;
  vUv = uv;
  float c = cos(rotation);
  float s = sin(rotation);
  mat3 transformMatrix = mat3(
    c, s, 0,
    -s, c, 0,
    0, 0, 1
  );
  vec3 pos = transformMatrix * vec3(a_vertexPosition, 1);
  gl_Position = vec4(pos, 1);
}
```

Combined with the following JavaScript code, you can make this square rotate:

```js
renderer.uniforms.rotation = 0.0

requestAnimationFrame(function update() {
	renderer.uniforms.rotation += 0.05
	requestAnimationFrame(update)
})
```

##### 6.2.2. Implementing Non-Fixed Frame Animation with Fragment Shader

Handle rotation in the fragment shader:

```js

#ifdef GL_ES
precision highp float;
#endif

varying vec2 vUv;
uniform vec4 color;
uniform float rotation;

void main() {
  vec2 st = 2.0 * (vUv - vec2(0.5));
  float c = cos(rotation);
  float s = sin(rotation);
  mat3 transformMatrix = mat3(
    c, s, 0,
    -s, c, 0,
    0, 0, 1
  );
  vec3 pos = transformMatrix * vec3(st, 1.0);
  float d1 = 1.0 - smoothstep(0.5, 0.505, abs(pos.x));
  float d2 = 1.0 - smoothstep(0.5, 0.505, abs(pos.y));
  gl_FragColor = d1 * d2 * color;
}
```

**The rotation animation implemented by the vertex shader and fragment shader is in exactly opposite directions.**

In the vertex shader, we directly change the vertex coordinates, so the rotation animation implemented in this way is consistent with the direction of the WebGL coordinate system (right-hand system), and the rotation is counterclockwise as the angle increases.

In the fragment shader, our drawing principle is to color through the distance field, so the rotation here actually changes the angle of the distance field instead of the graphic angle, and the finally drawn graphic is also relative to the distance field. Also, because the distance field rotates counterclockwise, the graphic rotates clockwise.

> Generally speaking, if the animation can be implemented using the vertex shader, we will try to implement it in the vertex shader. Because when drawing a frame of image, the computation amount of the vertex shader is much less than that of the fragment shader, so using the vertex shader consumes less performance.

However, implementing non-fixed frame animation in the fragment shader also has advantages. We can use fragment shader techniques such as repetition, randomness, noise, etc. to draw more complex effects.

For example, we slightly modify the above code, use the fractional and integer functions, and then use the previous meshing idea to implement a large number of repeated animations using the grid. This approach takes full advantage of the parallel efficiency of the GPU and has much higher performance than drawing graphics one by one in other ways.

#### 6.3. How to Implement Easing Functions and Non-Linear Interpolation in Shaders

It can be implemented using Shader matrix operations

Implement a shader that changes the position of the graphic by setting translation

```js
attribute vec2 a_vertexPosition;
attribute vec2 uv;

varying vec2 vUv;
uniform vec2 translation;

void main() {
  gl_PointSize = 1.0;
  vUv = uv;
  mat3 transformMatrix = mat3(
    1, 0, 0,
    0, 1, 0,
    translation, 1
  );
```

### 7. Drawing 3D Cubes with WebGL

### 8. Camera and View Matrix

In the initial state, the camera's reference coordinate system coincides with the world coordinate system. However, when we move or rotate the camera, the camera's reference coordinate system no longer aligns with the world coordinate system.

Since graphics are viewed from the camera's perspective, we need a transformation to convert world coordinates to camera coordinates. The matrix for this transformation is the View Matrix.

Generally speaking, there are two types of projection: orthographic projection and perspective projection.

**Orthographic projection**, also known as parallel projection, projects objects onto a cuboidal space (called a viewing frustum). The size of the projection remains constant regardless of the distance between the camera and the object.

**Perspective projection** is closer to human visual perception. Its projection rule is that objects closer to the camera appear larger, while those farther away appear smaller. Unlike orthographic projection (which uses a cuboidal viewing frustum), perspective projection uses a frustum (truncated pyramid) shape.

The `ortho` function calculates the orthographic projection matrix. Its parameters define the coordinate ranges of the viewing frustum along the x, y, and z axes, and its return value is the projection matrix.  
The `perspective` function calculates the perspective projection matrix. Its parameters include the near clipping plane (`near`), far clipping plane (`far`), field of view (`fov`), and aspect ratio (`aspect`), with the return value also being a projection matrix.

```js
// Calculate orthographic projection matrix
function ortho(out, left, right, bottom, top, near, far) {
	let lr = 1 / (left - right)
	let bt = 1 / (bottom - top)
	let nf = 1 / (near - far)
	out[0] = -2 * lr
	out[1] = 0
	out[2] = 0
	out[3] = 0
	out[4] = 0
	out[5] = -2 * bt
	out[6] = 0
	out[7] = 0
	out[8] = 0
	out[9] = 0
	out[10] = 2 * nf
	out[11] = 0
	out[12] = (left + right) * lr
	out[13] = (top + bottom) * bt
	out[14] = (far + near) * nf
	out[15] = 1
	return out
}

// Calculate perspective projection matrix
function perspective(out, fovy, aspect, near, far) {
	let f = 1.0 / Math.tan(fovy / 2)
	let nf = 1 / (near - far)
	out[0] = f / aspect
	out[1] = 0
	out[2] = 0
	out[3] = 0
	out[4] = 0
	out[5] = f
	out[6] = 0
	out[7] = 0
	out[8] = 0
	out[9] = 0
	out[10] = (far + near) * nf
	out[11] = -1
	out[12] = 0
	out[13] = 0
	out[14] = 2 * far * near * nf
	out[15] = 0
	return out
}
```

## three.js

Three.js is a 3D engine built on top of native WebGL.

![three.js Workflow](images/threejs-flow.png)

The yellow and green sections represent parts where three.js is involved—yellow for JavaScript and green for OpenGL ES.

- Assists in exporting model data;
- Automatically generates various matrices;
- Generates vertex shaders;
- Helps create materials and configure lighting;
- Generates fragment shaders based on the materials we set;
- Wraps WebGL's rasterization-based 2D API into human-readable 3D APIs.

![three.js Complete Runtime Process](images/threejs-running-process.png)

### 1. Program Structure

![Program Structure](images/threejs1.png)

**demo1: Create a Cube**

```js
// Create Scene object
const scene = new THREE.Scene()

// Create mesh model
const geometry = new THREE.BoxGeometry(100, 100, 100) // Create cube Geometry object
const material = new THREE.MeshLambertMaterial({
	color: 0x0000ff,
})

// Material object
const mesh = new THREE.Mesh(geometry, material) // Mesh object
scene.add(mesh) // Add mesh to scene

// Lighting setup
// Point light
const point = new THREE.PointLight(0xffffff)
point.position.set(400, 200, 300) // Point light position
scene.add(point) // Add point light to scene

// Ambient light
const ambient = new THREE.AmbientLight(0x444444)
scene.add(ambient)

// Camera setup
const width = window.innerWidth // Window width
const height = window.innerHeight // Window height
const k = width / height // Window aspect ratio
const s = 200 // 3D scene display range control coefficient (larger = wider range)

// Create camera object
const camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000)
camera.position.set(200, 300, 200) // Set camera position
camera.lookAt(scene.position) // Set camera direction (point to scene origin)

// Create renderer object
const renderer = new THREE.WebGLRenderer()
renderer.setSize(width, height) // Set render area size
renderer.setClearColor(0xb9d3ff, 1) // Set background color
document.body.appendChild(renderer.domElement) // Insert canvas into body

// Execute rendering (pass scene and camera as parameters)
renderer.render(scene, camera)
```

#### 1.1. Material

The code `var material = new THREE.MeshLambertMaterial({color:0x0000ff});` creates a material object for the cube using the `THREE.MeshLambertMaterial()` constructor. The constructor parameter is an object containing properties like color and transparency—this example only defines the `color` property (0x0000ff for blue). Changing it to 0x00ff00 will render a green cube. The color value uses the hexadecimal RGB color model.

##### Material Properties

- `color`: Material color (e.g., blue = 0x0000ff)
- `wireframe`: Renders geometry as wireframe (default: false)
- `opacity`: Transparency (0 = fully transparent, 1 = fully opaque)
- `transparent`: Enables transparency (default: false)

```js
// Translucent effect
var sphereMaterial = new THREE.MeshLambertMaterial({
	color: 0xff0000,
	opacity: 0.7,
	transparent: true,
})
```

##### Adding Specular Highlights

Surfaces under light exhibit reflection—macroscopically, the combined reflection effect of rough surfaces can be summarized by two models: diffuse reflection and specular reflection. The "highlight" concept in rendering refers to the local brightening from specular reflection in physical optics.

For three.js, diffuse and specular reflection correspond to `MeshLambertMaterial()` and `MeshPhongMaterial()` respectively. The three.js engine encapsulates WebGL lighting model algorithms, eliminating the need to implement them with native WebGL.

```js
// Add specular highlights
var sphereMaterial = new THREE.MeshPhongMaterial({
	color: 0x0000ff,
	specular: 0x4488ee,
	shininess: 12,
})
```

#### 1.2. Light

The code `var point = new THREE.PointLight(0xffffff);` creates a point light object using `THREE.PointLight()`, with 0xffffff defining light intensity. Changing it to 0x444444 will darken the cube's surface—intuitively, dimmer light results in darker surroundings. The three.js engine encapsulates all WebGL lighting model calculations.

##### Common Light Types

- `AmbientLight`: Ambient light
- `PointLight`: Point light
- `DirectionalLight`: Directional light (e.g., sunlight)
- `SpotLight`: Spot light

```js
// Ambient light (RGB components multiply with mesh material color)
var ambient = new THREE.AmbientLight(0x444444)
scene.add(ambient)
```

```js
// Point light
var point = new THREE.PointLight(0xffffff)
point.position.set(400, 200, 300) // Point light position
// Add to scene (required for lighting calculations during rendering)
scene.add(point)
```

#### 1.3. Camera

The code `var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);` creates an orthographic camera using `THREE.OrthographicCamera()`. The coefficient `s` (defined as 200) controls the scene display range—changing it to 300 will shrink the cube. This is analogous to photography: a larger framing range makes subjects appear smaller relative to the background.

`camera.position.set(200, 300, 200);` and `camera.lookAt(scene.position);` define the camera's position and viewing direction. Modifying the x-coordinate from 200 to 250 will change the cube's perspective—just as moving your camera changes the composition of a photo.

#### 1.4. Mouse Interaction with 3D Scene

To enable mouse interaction with the 3D scene, you can use the `OrbitControls.js` utility (found in `three.js-master\examples\js\controls`).

`OrbitControls.js` supports mouse (left/middle/right click) and keyboard (arrow keys) interactions. Replace `renderer.render(scene,camera)` with the following code:

```js
function render() {
	renderer.render(scene, camera) // Execute rendering
}
render()
var controls = new THREE.OrbitControls(camera, renderer.domElement) // Create controls object
controls.addEventListener('change', render) // Listen for mouse/keyboard events
```

The `THREE.OrbitControls()` constructor takes a camera object as a parameter. When you instantiate it with `new THREE.OrbitControls(camera, renderer.domElement)`, the browser automatically detects mouse/keyboard input and updates the camera's parameters (e.g., rotating the camera based on mouse drag). The `controls.addEventListener('change', render)` listener triggers re-rendering on every camera update, ensuring the scene reflects the camera's new position/angle.

### 2. Geometry Vertices

#### Geometry APIs

- Access vertex position data: `BufferGeometry.attributes.position`
- Access vertex color data: `BufferGeometry.attributes.color`
- Access vertex normal data: `BufferGeometry.attributes.normal`

#### 2.1. Vertex Position Data Parsing & Rendering

The following code creates a geometry with six vertices using three.js's `BufferGeometry` and `BufferAttribute` APIs:

```js
var geometry = new THREE.BufferGeometry() // Create BufferGeometry object
// Typed array for vertex data
var vertices = new Float32Array([
	0,
	0,
	0, // Vertex 1 coordinates
	50,
	0,
	0, // Vertex 2 coordinates
	0,
	100,
	0, // Vertex 3 coordinates
	0,
	0,
	10, // Vertex 4 coordinates
	0,
	0,
	100, // Vertex 5 coordinates
	50,
	0,
	10, // Vertex 6 coordinates
])
// Create attribute buffer object
var attribute = new THREE.BufferAttribute(vertices, 3) // 3 values per vertex (x/y/z)
// Set position attribute of geometry
geometry.attributes.position = attribute
```

##### Point Cloud (`Points`)

Pass the geometry to `Points` (instead of `Mesh`) to render the six vertices as square points:

```js
// Point rendering mode
var material = new THREE.PointsMaterial({
	color: 0xff0000,
	size: 10.0, // Point size in pixels
}) // Material object
var points = new THREE.Points(geometry, material) // Points object
scene.add(points) // Add points to scene
```

##### Line (`Line`)

```js
// Line rendering mode
var material = new THREE.LineBasicMaterial({
	color: 0xff0000, // Line color
}) // Material object
var line = new THREE.Line(geometry, material) // Line object
scene.add(line) // Add line to scene
```

#### 2.2. Vertex Color Data Interpolation

##### Assign a Color to Each Vertex

Render the six vertices with custom colors:

```js
var geometry = new THREE.BufferGeometry() // Create BufferGeometry object

// Typed array for vertex positions
var vertices = new Float32Array([
	0,
	0,
	0, // Vertex 1
	50,
	0,
	0, // Vertex 2
	0,
	100,
	0, // Vertex 3
	0,
	0,
	10, // Vertex 4
	0,
	0,
	100, // Vertex 5
	50,
	0,
	10, // Vertex 6
])
// Create attribute buffer object
var attribute = new THREE.BufferAttribute(vertices, 3) // 3 values per vertex
// Set position attribute
geometry.attributes.position = attribute

// Typed array for vertex colors
var colors = new Float32Array([
	1,
	0,
	0, // Vertex 1: red
	0,
	1,
	0, // Vertex 2: green
	0,
	0,
	1, // Vertex 3: blue
	1,
	1,
	0, // Vertex 4: yellow
	0,
	1,
	1, // Vertex 5: cyan
	1,
	0,
	1, // Vertex 6: magenta
])
// Set color attribute
geometry.attributes.color = new THREE.BufferAttribute(colors, 3) // 3 values (R/G/B) per vertex

// Material object
var material = new THREE.PointsMaterial({
	vertexColors: THREE.VertexColors, // Use vertex colors
	size: 10.0, // Point size
})

// Points object
var points = new THREE.Points(geometry, material)
scene.add(points)
```

#### 2.3. Vertex Normal Data for Lighting Calculations

In WebGL, normal vectors are required to calculate light incidence angles on surfaces. For three.js meshes (composed of triangles), normals are defined per vertex to represent surface orientation at each point.

```js
var normals = new Float32Array([
	0,
	0,
	1, // Vertex 1 normal
	0,
	0,
	1, // Vertex 2 normal
	0,
	0,
	1, // Vertex 3 normal
	0,
	1,
	0, // Vertex 4 normal
	0,
	1,
	0, // Vertex 5 normal
	0,
	1,
	0, // Vertex 6 normal
])
// Set normal attribute
geometry.attributes.normal = new THREE.BufferAttribute(normals, 3) // 3 values per normal
```

#### 2.4. Vertex Indices for Reusing Vertex Data

A rectangular mesh requires at least two triangles (6 vertices), but two vertices are shared between triangles. Vertex indices allow reusing shared vertices—only 4 unique vertices are needed for a rectangle.

##### `index` Attribute

Define a rectangle using `BufferGeometry.index` (vertex indices). Indices reference vertex positions/normals, reducing redundant data:

```js
var geometry = new THREE.BufferGeometry() // Empty BufferGeometry
// Vertex positions (4 unique vertices)
var vertices = new Float32Array([
	0,
	0,
	0, // Vertex 1
	80,
	0,
	0, // Vertex 2
	80,
	80,
	0, // Vertex 3
	0,
	80,
	0, // Vertex 4
])
var attribute = new THREE.BufferAttribute(vertices, 3)
geometry.attributes.position = attribute

// Vertex normals (4 normals)
var normals = new Float32Array([
	0,
	0,
	1, // Vertex 1 normal
	0,
	0,
	1, // Vertex 2 normal
	0,
	0,
	1, // Vertex 3 normal
	0,
	0,
	1, // Vertex 4 normal
])
geometry.attributes.normal = new THREE.BufferAttribute(normals, 3)
```

Define indices to form two triangles (reusing vertices 0 and 2):

```js
// Uint16Array for vertex indices
var indexes = new Uint16Array([
	// 3 indices per triangle
	0,
	1,
	2, // Triangle 1 (vertices 0,1,2)
	0,
	2,
	3, // Triangle 2 (vertices 0,2,3)
])
// Assign indices to geometry
geometry.index = new THREE.BufferAttribute(indexes, 1) // 1 value per index
```

| Typed Array  | Bits | Bytes | Description                  |
| ------------ | ---- | ----- | ---------------------------- |
| Int8Array    | 8    | 1     | Signed 8-bit integer         |
| Uint8Array   | 8    | 1     | Unsigned 8-bit integer       |
| Int16Array   | 16   | 2     | Signed 16-bit integer        |
| Uint16Array  | 16   | 2     | Unsigned 16-bit integer      |
| Int32Array   | 32   | 4     | Signed 32-bit integer        |
| Uint32Array  | 32   | 4     | Unsigned 32-bit integer      |
| Float32Array | 32   | 4     | 32-bit floating-point number |
| Float64Array | 64   | 8     | 64-bit floating-point number |

#### 2.5. `Face3` for Defining Triangular Faces (Geometry)

The `geometry.faces` property (for `Geometry`) is analogous to `BufferGeometry.index`—both use vertex indices to form triangles.

Create triangles using `Face3` (references vertices via indices from `geometry.vertices`):

```js
// Create triangular face (Face3)
var face1 = new THREE.Face3(0, 1, 2) // Indices 0,1,2 (triangle 1)
var face2 = new THREE.Face3(0, 2, 3) // Indices 0,2,3 (triangle 2)
```

##### Setting Triangle Normals

Normals can be defined per face or per vertex:

1. **Per-face normals**:

    ```js
    // Set face normal
    face2.normal = new THREE.Vector3(0, -1, 0)
    ```

2. **Per-vertex normals**:
    ```js
    // Vertex normals for face1
    var n1 = new THREE.Vector3(0, 0, -1) // Vertex 1 normal
    var n2 = new THREE.Vector3(0, 0, -1) // Vertex 2 normal
    var n3 = new THREE.Vector3(0, 0, -1) // Vertex 3 normal
    // Assign vertex normals to face1
    face1.vertexNormals.push(n1, n2, n3)
    ```

##### Setting Triangle Colors

Colors can be defined per face or per vertex (interpolation for gradient effects):

1. **Per-face color**:

    ```js
    face1.color = new THREE.Color(0xffff00) // Yellow
    ```

2. **Per-vertex color (gradient)**:
    ```js
    face1.vertexColors = [
    	new THREE.Color(0xffff00), // Vertex 1: yellow
    	new THREE.Color(0xff00ff), // Vertex 2: magenta
    	new THREE.Color(0x00ffff), // Vertex 3: cyan
    ]
    ```

#### 2.6. Accessing Geometry Data

`BoxGeometry` automatically generates vertices, normals, and faces:

```js
var geometry = new THREE.BoxGeometry(100, 100, 100) // Cube geometry
console.log(geometry)
console.log('Vertex positions:', geometry.vertices)
console.log('Triangular faces:', geometry.faces)
```

For `BufferGeometry` (e.g., `PlaneBufferGeometry`):

```js
var geometry = new THREE.PlaneBufferGeometry(100, 100) // Plane
console.log(geometry)
console.log('Vertex positions:', geometry.attributes.position)
console.log('Indices:', geometry.index)
```

#### 2.7. Geometry Transformations (Scale/Rotate/Translate)

![Geometry Transformations](images/threejs2.png)

Methods like `.scale()`, `.translate()`, and `.rotateX()` modify vertex positions directly:

```js
var geometry = new THREE.BoxGeometry(100, 100, 100) // Cube
geometry.scale(2, 2, 2) // Scale by 2x in all axes
geometry.translate(50, 0, 0) // Translate 50 units along X
geometry.rotateX(Math.PI / 4) // Rotate 45° around X
geometry.center() // Center geometry
console.log(geometry.vertices) // Modified vertices
```

> **Note**: Transforming a `Mesh` (e.g., `mesh.scale.set()`) achieves the same visual effect but does **not** modify geometry vertices—it updates the mesh's local/world matrix instead:
>
> ```js
> // Scale geometry (modifies vertices)
> geometry.scale(0.5, 1.5, 2)
>
> // Scale mesh (no vertex changes)
> mesh.scale.set(0.5, 1.5, 2)
> ```

### 3. Materials

All materials in three.js are wrappers for WebGL shader code.

![Materials](images/threejs3.png)

#### 3.1. `PointsMaterial` (Point Material)

The `.size` property defines the pixel size of each vertex (rendered as squares):

```js
var geometry = new THREE.SphereGeometry(100, 25, 25) // Sphere
var material = new THREE.PointsMaterial({
	color: 0x0000ff, // Blue
	size: 3, // Point size
})
var point = new THREE.Points(geometry, material) // Points object
scene.add(point)
```

#### 3.2. Line Materials

Two line materials are available: `LineBasicMaterial` (solid lines) and `LineDashedMaterial` (dashed lines):

```js
var geometry = new THREE.SphereGeometry(100, 25, 25) // Sphere

// Solid line material
var material = new THREE.LineBasicMaterial({
	color: 0x0000ff, // Blue
})
var line = new THREE.Line(geometry, material)
scene.add(line)

// Dashed line material
var material = new THREE.LineDashedMaterial({
	color: 0x0000ff, // Blue
	dashSize: 10, // Dash length (default: 3)
	gapSize: 5, // Gap length (default: 1)
})
var line = new THREE.Line(geometry, material)
line.computeLineDistances() // Required for dashed lines
```

#### 3.3. Mesh Materials

Materials for mesh-based models:

```js
// Basic material (unaffected by lighting, no shading)
var material = new THREE.MeshBasicMaterial({
	color: 0x0000ff, // Blue
})

// Lambert material (diffuse reflection, shading)
var material = new THREE.MeshLambertMaterial({
	color: 0x00ff00, // Green
})

// Phong material (diffuse + specular reflection, highlights)
var material = new THREE.MeshPhongMaterial({
	color: 0xff0000, // Red
	specular: 0x444444, // Highlight color
	shininess: 20, // Highlight intensity (default: 30)
})
```

#### 3.4. Material-Model Compatibility

![Material-Model Compatibility](images/threejs4.png)

#### 3.5. `.side` Property

Defines which face(s) to render (front/back/double-sided):

```js
var material = new THREE.MeshBasicMaterial({
	color: 0xdd00ff, // Magenta
	side: THREE.DoubleSide, // Render both sides (default: FrontSide)
})
```

#### 3.6. Transparency (`opacity`)

The `.opacity` property (0 = transparent, 1 = opaque) requires `transparent: true` to take effect:

```js
// Constructor parameters
var material = new THREE.MeshPhongMaterial({
	color: 0x220000, // Dark red
	transparent: true, // Enable transparency
	opacity: 0.4, // 40% opaque
})

// Runtime modification
material.transparent = true
material.opacity = 0.4
```

### 4. Points, Lines, and Meshes

Points, lines, and meshes all use geometry + material but differ in how vertices are rendered:

![Points/Lines/Meshes](images/threejs5.png)

#### 4.1. Points (`Points`)

Renders each vertex as a square (size controlled by `PointsMaterial.size`):

```js
var geometry = new THREE.BoxGeometry(100, 100, 100) // Cube
var material = new THREE.PointsMaterial({
	color: 0xff0000, // Red
	size: 5.0, // Point size
})
var points = new THREE.Points(geometry, material) // Points object
```

#### 4.2. Lines (`Line`)

Connects vertices with lines. Three line types are available:

- `Line`: Connects vertices in sequence (no closure)
- `LineLoop`: Connects vertices and closes the shape (last → first)
- `LineSegments`: Renders disjoint segments (1-2, 3-4, etc.)

```js
var geometry = new THREE.BoxGeometry(100, 100, 100) // Cube
var material = new THREE.LineBasicMaterial({
	color: 0xff0000, // Red
})
var line = new THREE.Line(geometry, material) // Line object
```

#### 4.3. Meshes (`Mesh`)

Renders geometry as connected triangles (default). Enable `wireframe` to view the triangle structure:

```js
var geometry = new THREE.BoxGeometry(100, 100, 100) // Cube
var material = new THREE.MeshLambertMaterial({
	color: 0x0000ff, // Blue
	wireframe: true, // Wireframe mode
})
var mesh = new THREE.Mesh(geometry, material) // Mesh object
```

#### 4.4. Cloning (`clone()`) and Copying (`copy()`)

![Clone/Copy](images/threejs6.png)

1. **Copy (`copy()`)**:

    ```js
    var p1 = new THREE.Vector3(1.2, 2.6, 3.2)
    var p2 = new THREE.Vector3(0, 0, 0)
    p2.copy(p1) // p2 = (1.2, 2.6, 3.2)
    console.log(p2)
    ```

2. **Clone (`clone()`)**:

    ```js
    var p1 = new THREE.Vector3(1.2, 2.6, 3.2)
    var p2 = p1.clone() // p2 = (1.2, 2.6, 3.2)
    console.log(p2)
    ```

3. **Mesh Cloning**:

    ```js
    var box = new THREE.BoxGeometry(10, 10, 10) // Cube geometry
    var material = new THREE.MeshLambertMaterial({ color: 0x0000ff }) // Blue material
    var mesh = new THREE.Mesh(box, material) // Original mesh
    var mesh2 = mesh.clone() // Cloned mesh
    mesh.translateX(20) // Translate original (clone unaffected)
    scene.add(mesh, mesh2)
    ```

    > **Note**: Cloning a mesh shares geometry/material references by default. Scaling the geometry (`box.scale(1.5,1.5,1.5)`) affects both meshes. Geometry cloning performs a deep copy of vertex data (unlike mesh cloning).

### 2.5. Lights

![Lights](images/threejs7.png)

#### 2.5.1. AmbientLight

Ambient light has no direction and uniformly brightens/darkens surfaces (unlike directional lights, which create gradients):

```js
// Ambient light (RGB components multiply with material color)
var ambient = new THREE.AmbientLight(0x444444) // Dim gray
scene.add(ambient)
```

#### 2.5.2. PointLight

Point lights emit light in all directions (like light bulbs). Position (`position`) affects which surfaces are lit and their brightness (attenuation with distance):

```js
// Point light (white)
var point = new THREE.PointLight(0xffffff)
point.position.set(400, 200, 300) // Position
scene.add(point)
```

#### 2.5.3. DirectionalLight

Directional light (e.g., sunlight) has parallel rays—incidence angle is uniform across a surface. Defined by `position` and `target` (direction = `target.position - position`):

```js
// Directional light (white, intensity = 1)
var directionalLight = new THREE.DirectionalLight(0xffffff, 1)
directionalLight.position.set(80, 100, 50) // Light position
directionalLight.target = mesh2 // Point light at mesh2 (default: origin)
scene.add(directionalLight)
```

#### 2.5.4. SpotLight

Spotlights emit light in a cone (defined by `angle`). Direction is set via `position` and `target`:

```js
// Spot light (white)
var spotLight = new THREE.SpotLight(0xffffff)
spotLight.position.set(200, 200, 200) // Position
spotLight.target = mesh2 // Target
spotLight.angle = Math.PI / 6 // Cone angle (30°)
scene.add(spotLight)
```

#### 2.5.5. Lighting Calculation Algorithm

Three.js multiplies material color (`mesh.material.color`) with light color (`light.color`) and the cosine of the incidence angle (diffuse reflection):

```
Diffuse color = Material color × Light color × cos(θ)
```

Example (red light + white material = red mesh):

```js
// White material (reflects all colors)
var material = new THREE.MeshLambertMaterial({ color: 0xffffff })
var mesh = new THREE.Mesh(new THREE.BoxGeometry(100, 100, 100), material)
scene.add(mesh)

// Red ambient light
var ambient = new THREE.AmbientLight(0x440000)
scene.add(ambient)

// Red point light
var point = new THREE.PointLight(0xff0000)
point.position.set(400, 200, 300)
scene.add(point)
```

#### 2.5.6. Directional Light Shadow Calculation

Shadows require three settings:

1. **Caster**: Object that casts shadows (`castShadow = true`)
2. **Receiver**: Object that receives shadows (`receiveShadow = true`)
3. **Light**: Enable shadow calculation for the light (`castShadow = true`)

```js
// Create plane for shadow reception
var planeGeometry = new THREE.PlaneGeometry(300, 200)
var planeMaterial = new THREE.MeshLambertMaterial({ color: 0x999999 })
var planeMesh = new THREE.Mesh(planeGeometry, planeMaterial)
planeMesh.rotateX(-Math.PI / 2) // Rotate to horizontal
planeMesh.position.y = -50 // Lower plane
planeMesh.receiveShadow = true // Enable shadow reception
scene.add(planeMesh)

// Directional light
var directionalLight = new THREE.DirectionalLight(0xffffff, 1)
directionalLight.position.set(60, 100, 40)
directionalLight.castShadow = true // Enable shadow calculation
// Shadow camera frustum (tight fit around objects for quality)
directionalLight.shadow.camera.near = 0.5
directionalLight.shadow.camera.far = 300
directionalLight.shadow.camera.left = -50
directionalLight.shadow.camera.right = 50
directionalLight.shadow.camera.top = 200
directionalLight.shadow.camera.bottom = -100
// Improve shadow quality (higher = more GPU usage)
// directionalLight.shadow.mapSize.set(1024, 1024)
scene.add(directionalLight)

// Enable shadow casting for the cube
mesh.castShadow = true
```

#### 2.5.7. SpotLight Shadow Calculation

Similar to directional light, with frustum defined by `fov` (instead of left/right/top/bottom):

```js
// Spot light
var spotLight = new THREE.SpotLight(0xffffff)
spotLight.position.set(50, 90, 50)
spotLight.angle = Math.PI / 6 // 30° cone
spotLight.castShadow = true // Enable shadows
// Shadow camera frustum
spotLight.shadow.camera.near = 1
spotLight.shadow.camera.far = 300
spotLight.shadow.camera.fov = 20 // Field of view for shadow camera
scene.add(spotLight)
```

### 2.6. Hierarchical Models (Tree Structure)

Hierarchical models (e.g., a robot) organize objects into parent-child relationships (e.g., head → eyes, arm → hand → fingers).

#### 2.6.1. Group Object & Hierarchical Models

![Hierarchy](images/threejs8.png)

Create a group to organize meshes (child objects inherit parent transformations):

```js
// Create two cubes
var geometry = new THREE.BoxGeometry(20, 20, 20)
var material = new THREE.MeshLambertMaterial({ color: 0x0000ff })
var group = new THREE.Group() // Group object
var mesh1 = new THREE.Mesh(geometry, material)
var mesh2 = new THREE.Mesh(geometry, material)
mesh2.translateX(25) // Offset mesh2

// Add meshes to group (children)
group.add(mesh1, mesh2)
// Add group to scene (parent: scene)
scene.add(group)

// Transform group (children inherit)
group.translateY(100) // Translate Y
group.scale.set(4, 4, 4) // Scale
group.rotateY(Math.PI / 6) // Rotate Y
```

1. **Access Children (`children`)**:

    ```js
    console.log(group.children) // [mesh1, mesh2]
    console.log(scene.children) // [group, ambient, point]
    ```

2. **Add/Remove Objects**:
    ```js
    // Add multiple objects
    group.add(mesh1, mesh2)
    // Remove objects
    scene.remove(light, group)
    ```

#### 2.6.2. Naming, Finding, and Traversing Nodes

1. **Name Nodes (`name`)**:

    ```js
    mesh.name = 'eye'
    group.name = 'head'
    ```

2. **Traverse Nodes (`traverse()`)**:

    ```js
    scene.traverse(function (obj) {
    	if (obj.type === 'Group') console.log(obj.name) // Log groups
    	if (obj.type === 'Mesh') {
    		console.log('  ' + obj.name) // Log meshes
    		obj.material.color.set(0xffff00) // Set yellow
    	}
    	if (obj.name === 'leftEye' || obj.name === 'rightEye') {
    		obj.material.color.set(0x000000) // Set black
    	}
    })
    ```

3. **Find Nodes**:
    ```js
    // Find by ID
    var idNode = scene.getObjectById(4)
    // Find by name (returns first match)
    var nameNode = scene.getObjectByName('leftLeg')
    nameNode.material.color.set(0xff0000) // Set red
    ```

#### 2.6.3. Coordinates

- **Local Coordinates**: `mesh.position` (relative to parent)
- **World Coordinates**: `mesh.getWorldPosition()` (absolute position)

```js
// Local coordinates (mesh: (50,0,0), group: (50,0,0))
console.log('Local:', mesh.position) // (50,0,0)

// Update world matrices (required before rendering)
scene.updateMatrixWorld(true)
// Get world coordinates (50+50=100 on X)
var worldPosition = new THREE.Vector3()
mesh.getWorldPosition(worldPosition)
console.log('World:', worldPosition) // (100,0,0)
```

### 2.7. Geometry, Curves, and 3D Models

#### 2.7.1. Geometry

All geometries inherit from `Geometry` or `BufferGeometry` (convertible between types).

![Geometry](images/threejs9.png)

#### 2.7.2. Curves

Curves generate vertices along a path (higher segmentation = smoother curves).

![Curves](images/threejs10.png)

**ArcCurve**:

```js
// ArcCurve(centerX, centerY, radius, startAngle, endAngle, clockwise)
var arc = new THREE.ArcCurve(0, 0, 100, 0, 2 * Math.PI) // Full circle
```

| Parameter                  | Description                        |
| -------------------------- | ---------------------------------- |
| `aX`, `aY`                 | Arc center coordinates             |
| `aRadius`                  | Arc radius                         |
| `aStartAngle`, `aEndAngle` | Start/end angles (radians)         |
| `aClockwise`               | Clockwise drawing (default: false) |

**`getPoints()` (Curve Method)**:
Generates vertices along the curve (higher count = smoother):

```js
var points = arc.getPoints(50) // 50 segments → 51 vertices (Vector2 array)
```

**`setFromPoints()` (Geometry Method)**:
Assigns curve points to geometry vertices:

```js
geometry.setFromPoints(points) // Update geometry.vertices
// For BufferGeometry: updates geometry.attributes.position
```

#### 2.7.3. Spline & Bezier Curves

**CatmullRomCurve3 (Spline)**:
Smooth curve through control points:

```js
var curve = new THREE.CatmullRomCurve3([
	new THREE.Vector3(-50, 20, 90),
	new THREE.Vector3(-10, 40, 40),
	new THREE.Vector3(0, 0, 0),
	new THREE.Vector3(60, -60, 0),
	new THREE.Vector3(70, 0, 80),
])
var points = curve.getPoints(100) // 100 segments
geometry.setFromPoints(points)
```

**Quadratic/Cubic BezierCurve3**:
Curves defined by start/end points and control points:

```js
// Quadratic (1 control point)
var p1 = new THREE.Vector3(-80, 0, 0) // Start
var p2 = new THREE.Vector3(20, 100, 0) // Control
var p3 = new THREE.Vector3(80, 0, 0) // End
var curve = new THREE.QuadraticBezierCurve3(p1, p2, p3)

// Cubic (2 control points)
var p1 = new THREE.Vector3(-80, 0, 0) // Start
var p2 = new THREE.Vector3(-40, 100, 0) // Control 1
var p3 = new THREE.Vector3(40, 100, 0) // Control 2
var p4 = new THREE.Vector3(80, 0, 0) // End
var curve = new THREE.CubicBezierCurve3(p1, p2, p3, p4)
```

#### 2.7.4. Composite Curves (`CurvePath`)

Combine multiple curves into one path:

```js
// U-shaped path (arc + two lines)
var R = 80 // Arc radius
var arc = new THREE.ArcCurve(0, 0, R, 0, Math.PI, true) // Half-circle
var line1 = new THREE.LineCurve(new THREE.Vector2(R, 200), new THREE.Vector2(R, 0))
var line2 = new THREE.LineCurve(new THREE.Vector2(-R, 0), new THREE.Vector2(-R, 200))

// Composite path
var curvePath = new THREE.CurvePath()
curvePath.curves.push(line1, arc, line2)

// Generate points
var points = curvePath.getPoints(200)
geometry.setFromPoints(points)
```

#### 2.7.5. TubeGeometry (Extrude Curve into Tube)

Creates a tube along a curve:

```js
// TubeGeometry(path, tubularSegments, radius, radiusSegments, closed)
var path = new THREE.CatmullRomCurve3([
	new THREE.Vector3(-10, -50, -50),
	new THREE.Vector3(10, 0, 0),
	new THREE.Vector3(8, 50, 50),
	new THREE.Vector3(-5, 0, 100),
])
var geometry = new THREE.TubeGeometry(path, 40, 2, 25) // 40 segments, 2 units radius
```

#### 2.7.6. LatheGeometry (Revolution Surface)

Creates a 3D surface by rotating 2D points around the Y-axis:

```js
// LatheGeometry(points, segments, phiStart, phiLength)
var points = [new THREE.Vector2(50, 60), new THREE.Vector2(25, 0), new THREE.Vector2(50, -60)]
var geometry = new THREE.LatheGeometry(points, 30) // 30 segments
var material = new THREE.MeshPhongMaterial({
	color: 0x0000ff,
	side: THREE.DoubleSide, // Double-sided
	wireframe: true, // Wireframe mode
})
var mesh = new THREE.Mesh(geometry, material)
scene.add(mesh)
```

**Spline Interpolation**:
Smooth the rotation path with spline interpolation:

```js
var shape = new THREE.Shape()
var points = [new THREE.Vector2(50, 60), new THREE.Vector2(25, 0), new THREE.Vector2(50, -60)]
shape.splineThru(points) // Spline interpolation
var splinePoints = shape.getPoints(20) // 20 segments
var geometry = new THREE.LatheGeometry(splinePoints, 30)
```

#### 2.7.7. Shape & ShapeGeometry (Filled Contours)

**Fill a Polygon**:

```js
// Define pentagon vertices
var points = [
	new THREE.Vector2(-50, -50),
	new THREE.Vector2(-60, 0),
	new THREE.Vector2(0, 50),
	new THREE.Vector2(60, 0),
	new THREE.Vector2(50, -50),
	new THREE.Vector2(-50, -50), // Close shape
]
var shape = new THREE.Shape(points) // Contour
var geometry = new THREE.ShapeGeometry(shape, 25) // Fill with triangles
```

**Nested Contours (Holes)**:

```js
var shape = new THREE.Shape()
// Outer contour (circle)
shape.arc(0, 0, 100, 0, 2 * Math.PI)
// Inner contours (holes)
var path1 = new THREE.Path()
path1.arc(0, 0, 40, 0, 2 * Math.PI) // Center hole
var path2 = new THREE.Path()
path2.arc(80, 0, 10, 0, 2 * Math.PI) // Right hole
var path3 = new THREE.Path()
path3.arc(-80, 0, 10, 0, 2 * Math.PI) // Left hole
// Add holes
shape.holes.push(path1, path2, path3)
// Create geometry
var geometry = new THREE.ShapeGeometry(shape, 25)
```

### 2.8. Textures

#### 2.8.1. Create a Texture

Load an image with `TextureLoader` to create a `Texture` object (assign to `material.map`):

```js
// Plane geometry (match image aspect ratio)
var geometry = new THREE.PlaneGeometry(204, 102)
// Texture loader
var textureLoader = new THREE.TextureLoader()
// Load image (returns Texture object)
textureLoader.load('Earth.png', function (texture) {
	var material = new THREE.MeshLambertMaterial({
		map: texture, // Assign texture
	})
	var mesh = new THREE.Mesh(geometry, material)
	scene.add(mesh)
	// Render after load
	render()
})
```

#### 2.8.2. UV Coordinates

UV coordinates map texture pixels to geometry vertices (two sets: one for color/normal maps, one for light maps).

**Modify UVs**:

```js
// Set all UVs to (0.4, 0.4) (samples single texture pixel)
geometry.faceVertexUvs[0].forEach(elem => {
	elem.forEach(v2 => v2.set(0.4, 0.4))
})

// Assign full texture to specific triangles (4x4 plane)
var t0 = new THREE.Vector2(0, 1) // Bottom-left
var t1 = new THREE.Vector2(0, 0) // Bottom-right
var t2 = new THREE.Vector2(1, 0) // Top-right
var t3 = new THREE.Vector2(1, 1) // Top-left
var uv1 = [t0, t1, t3] // Triangle 1 UVs
var uv2 = [t1, t2, t3] // Triangle 2 UVs
// Assign to 5th/6th triangles
geometry.faceVertexUvs[0][4] = uv1
geometry.faceVertexUvs[0][5] = uv2
```

#### 2.8.3. Multi-Materials & Material Indices

Assign different materials to different faces (array of materials):

```js
var geometry = new THREE.BoxGeometry(100, 100, 100) // Cube
// Material 1 (yellow)
var material1 = new THREE.MeshPhongMaterial({ color: 0xffff3f })
// Material 2 (Earth texture)
var textureLoader = new THREE.TextureLoader()
var texture = textureLoader.load('Earth.png')
var material2 = new THREE.MeshLambertMaterial({ map: texture })
// Material array (assign to cube faces)
var materials = [material2, material1, material1, material1, material1, material1]
// Create mesh with multi-materials
var mesh = new THREE.Mesh(geometry, materials)
scene.add(mesh)
```

#### 2.8.4. Texture Repeat/Offset/Rotation

1. **Repeat**:

    ```js
    texture.wrapS = THREE.RepeatWrapping // Horizontal repeat
    texture.wrapT = THREE.RepeatWrapping // Vertical repeat
    texture.repeat.set(4, 2) // 4x horizontal, 2x vertical
    ```

2. **Offset**:

    ```js
    texture.offset = new THREE.Vector2(0.3, 0.1) // Shift texture
    ```

3. **Rotation**:
    ```js
    texture.rotation = Math.PI / 4 // Rotate 45°
    texture.center = new THREE.Vector2(0.5, 0.5) // Rotate around center
    ```

#### 2.8.5. DataTexture (Dynamic Textures)

Create textures from pixel data (no image file):

```js
var geometry = new THREE.PlaneGeometry(128, 128)
// Generate random pixel data (RGB)
var width = 32,
	height = 32
var size = width * height
var data = new Uint8Array(size * 3) // 3 bytes per pixel (R/G/B)
for (let i = 0; i < size * 3; i += 3) {
	data[i] = 255 * Math.random() // R
	data[i + 1] = 255 * Math.random() // G
	data[i + 2] = 255 * Math.random() // B
}
// Create DataTexture (RGB format)
var texture = new THREE.DataTexture(data, width, height, THREE.RGBFormat)
texture.needsUpdate = true // Force update
// Material
var material = new THREE.MeshPhongMaterial({ map: texture })
var mesh = new THREE.Mesh(geometry, material)
scene.add(mesh)
```

#### 2.8.6. Advanced Textures

1. **Normal Map (`normalMap`)**: Adds surface detail (bump mapping):

    ```js
    var textureNormal = textureLoader.load('normal.jpg')
    var material = new THREE.MeshPhongMaterial({
    	normalMap: textureNormal,
    	normalScale: new THREE.Vector2(3, 3), // Detail intensity
    })
    ```

2. **Bump Map (`bumpMap`)**: Grayscale height map (simpler than normal maps):

    ```js
    var textureBump = textureLoader.load('bump.jpg')
    var material = new THREE.MeshPhongMaterial({
    	map: texture, // Base texture
    	bumpMap: textureBump,
    	bumpScale: 3, // Height scale
    })
    ```

3. **Light Map (`lightMap`)**: Pre-baked shadows/lighting:

    ```js
    planeGeometry.faceVertexUvs[1] = planeGeometry.faceVertexUvs[0] // Use UVs for light map
    var textureLight = textureLoader.load('shadow.png')
    var planeMaterial = new THREE.MeshLambertMaterial({
    	color: 0x999999,
    	lightMap: textureLight,
    })
    ```

4. **Specular Map (`specularMap`)**: Controls highlight intensity per pixel:

    ```js
    var textureSpecular = textureLoader.load('specular.jpg')
    var material = new THREE.MeshPhongMaterial({
    	map: texture, // Base texture
    	specularMap: textureSpecular,
    	shininess: 30, // Highlight sharpness
    })
    ```

5. **Environment Map (`envMap`)**: Reflective textures (cube maps):
    ```js
    var cubeLoader = new THREE.CubeTextureLoader()
    cubeLoader.setPath('env/') // Base path
    var envMap = cubeLoader.load([
    	'px.jpg',
    	'nx.jpg', // X+ / X-
    	'py.jpg',
    	'ny.jpg', // Y+ / Y-
    	'pz.jpg',
    	'nz.jpg', // Z+ / Z-
    ])
    var material = new THREE.MeshPhongMaterial({
    	envMap: envMap, // Environment map
    	reflectivity: 0.8, // Reflection strength
    })
    ```

### 2.9. Cameras

![Cameras](images/threejs11.png)

#### 2.9.1. Orthographic vs. Perspective Cameras

- **Orthographic**: No perspective distortion (size constant with distance).
- **Perspective**: Mimics human vision (objects shrink with distance).

##### OrthographicCamera

```js
// OrthographicCamera(left, right, top, bottom, near, far)
var width = window.innerWidth,
	height = window.innerHeight
var k = width / height // Aspect ratio
var s = 200 // Scale
var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000)
```

| Parameter      | Description                        |
| -------------- | ---------------------------------- |
| `left`/`right` | Left/right frustum bounds          |
| `top`/`bottom` | Top/bottom frustum bounds          |
| `near`         | Near clipping plane (default: 0.1) |
| `far`          | Far clipping plane (default: 1000) |

> **Note**: Ensure `(right-left)/(top-bottom) = window width/height` to avoid stretching.

##### PerspectiveCamera

```js
// PerspectiveCamera(fov, aspect, near, far)
var camera = new THREE.PerspectiveCamera(60, width / height, 1, 1000)
camera.position.set(200, 300, 200) // Camera position
camera.lookAt(scene.position) // Point at scene origin
```

| Parameter | Description                              | Default                                |
| --------- | ---------------------------------------- | -------------------------------------- |
| `fov`     | Field of view (degrees, 60-90 for games) | 45                                     |
| `aspect`  | Aspect ratio (window width/height)       | `window.innerWidth/window.innerHeight` |
| `near`    | Near clipping plane                      | 0.1                                    |
| `far`     | Far clipping plane                       | 1000                                   |

### 2.10. TextGeometry (3D Text)

Create 3D text with `TextGeometry` (requires font files from `three.js-master/examples/fonts`):

```js
// Load font (JSON format)
var loader = new THREE.FontLoader()
loader.load('helvetiker_regular.typeface.json', function (font) {
	// Create text geometry
	var textGeo = new THREE.TextGeometry('Hello', {
		font: font,
		size: 1, // Font size
		height: 1, // Extrusion depth
		curveSegments: 12, // Smooth curves
		bevelEnabled: true, // Beveled edges
		bevelThickness: 0.1,
		bevelSize: 0.05,
	})
	// Create mesh
	var mesh = new THREE.Mesh(
		textGeo,
		new THREE.MeshBasicMaterial({
			color: 0xffff00,
			wireframe: true,
		}),
	)
	scene.add(mesh)
	// Render
	renderer.render(scene, camera)
})
```

**Chinese Fonts**:

1. Download a Chinese TTF font (e.g., Source Han Sans).
2. Convert to JSON via [facetype.js](https://gero3.github.io/facetype.js/).
3. Load the JSON font file.

### 2.11. Animation System

Three.js provides keyframe animation APIs: `KeyframeTrack`, `AnimationClip`, `AnimationAction`, `AnimationMixer`.

#### KeyframeTrack

Defines animated property values over time:

```js
// Position track (Box moves from (0,0,0) to (150,0,0) over 10s)
var posTrack = new THREE.KeyframeTrack('Box.position', [0, 10], [0, 0, 0, 150, 0, 0])
// Color track (Box changes from red to blue over 10-20s)
var colorTrack = new THREE.KeyframeTrack('Box.material.color', [10, 20], [1, 0, 0, 0, 0, 1])
// Scale track (Sphere scales 3x over 0-20s)
var scaleTrack = new THREE.KeyframeTrack('Sphere.scale', [0, 20], [1, 1, 1, 3, 3, 3])
```

#### AnimationClip

Groups tracks into a reusable animation:

```js
var clip = new THREE.AnimationClip('default', 20, [posTrack, colorTrack, scaleTrack])
```

#### AnimationMixer & AnimationAction

Play/control animations:

```js
// Mixer (binds to object/group)
var mixer = new THREE.AnimationMixer(group)
// Action (controls clip playback)
var action = mixer.clipAction(clip)
action.timeScale = 1 // Playback speed (1 = normal)
action.loop = THREE.LoopRepeat // Loop mode
action.play() // Start animation

// Update mixer in render loop
function animate() {
	requestAnimationFrame(animate)
	mixer.update(0.016) // Delta time (≈1/60s)
	renderer.render(scene, camera)
}
animate()
```

### 2.12. Skeletal & Morph Target Animation

#### Skeletal Animation (SkinnedMesh)

Skeletal animation uses `Bone`, `Skeleton`, and `SkinnedMesh` (typically loaded from 3D models, but can be created programmatically):

1. **Create Bones**:

    ```js
    var bone1 = new THREE.Bone() // Root bone
    var bone2 = new THREE.Bone() // Child of bone1
    var bone3 = new THREE.Bone() // Child of bone2
    bone1.add(bone2)
    bone2.add(bone3)
    // Position bones
    bone2.position.y = 60
    bone3.position.y = 40
    ```

2. **Create Skeleton**:

    ```js
    var skeleton = new THREE.Skeleton([bone1, bone2, bone3])
    ```

3. **Bind to SkinnedMesh**:
    ```js
    var geometry = new THREE.CylinderGeometry(5, 5, 120, 16) // Long cylinder
    // Assign skin weights/indices (define bone influence on vertices)
    for (var i = 0; i < geometry.vertices.length; i++) {
    	var v = geometry.vertices[i]
    	if (v.y <= 60) {
    		geometry.skinIndices.push(new THREE.Vector4(0, 0, 0, 0)) // Bone1
    		geometry.skinWeights.push(new THREE.Vector4(1 - v.y / 60, 0, 0, 0))
    	} else if (v.y <= 100) {
    		geometry.skinIndices.push(new THREE.Vector4(1, 0, 0, 0)) // Bone2
    		geometry.skinWeights.push(new THREE.Vector4(1 - (v.y - 60) / 40, 0, 0, 0))
    	} else {
    		geometry.skinIndices.push(new THREE.Vector4(2, 0, 0, 0)) // Bone3
    		geometry.skinWeights.push(new THREE.Vector4(1 - (v.y - 100) / 20, 0, 0, 0))
    	}
    }
    // Create SkinnedMesh
    var skinnedMesh = new THREE.SkinnedMesh(
    	geometry,
    	new THREE.MeshPhongMaterial({
    		color: 0x00ff00,
    		skinning: true, // Enable skinning
    	}),
    )
    skinnedMesh.add(bone1) // Add root bone to mesh
    skinnedMesh.bind(skeleton) // Bind skeleton
    scene.add(skinnedMesh)
    ```

### 2.13. Audio

Three.js wraps the Web Audio API with `Audio`, `PositionalAudio`, `AudioListener`, `AudioAnalyser`, and `AudioLoader`.

**Frequency Visualization**:

```js
// Create listener (required for audio)
var listener = new THREE.AudioListener()
camera.add(listener)

// Load audio
var audioLoader = new THREE.AudioLoader()
audioLoader.load('sound.mp3', function (buffer) {
	// Create positional audio
	var audio = new THREE.PositionalAudio(listener)
	audio.setBuffer(buffer)
	audio.setRefDistance(10) // Distance for full volume
	mesh.add(audio)
	audio.play()

	// Analyze frequency data
	var analyser = new THREE.AudioAnalyser(audio, 256)
	// Update in render loop
	function animate() {
		requestAnimationFrame(animate)
		var freqData = analyser.getFrequencyData() // Frequency array
		// Modify meshes based on frequency
		group.children.forEach((mesh, i) => {
			mesh.scale.y = freqData[i] / 80
			mesh.material.color.r = freqData[i] / 200
		})
		renderer.render(scene, camera)
	}
	animate()
})
```

### 2.14. Model Loading

#### Export Three.js Models

Use `.toJSON()` to export geometry/material/scene data:

```js
// Export geometry
var geometry = new THREE.BoxGeometry(100, 100, 100)
console.log(JSON.stringify(geometry))

// Export scene
scene.add(mesh)
console.log(JSON.stringify(scene))
```

#### Load Custom Models

Three.js provides loaders for common formats (GLTF, OBJ, FBX) in `three.js-master/examples/js/loaders`:

```js
// Load BufferGeometry from JSON
var loader = new THREE.BufferGeometryLoader()
loader.load('cube.json', function (geometry) {
	var material = new THREE.MeshLambertMaterial({ color: 0x0000ff })
	var mesh = new THREE.Mesh(geometry, material)
	scene.add(mesh)
})
```

### 2.15. Performance Analysis & Optimization

#### 2.15.1. Stats.js (Performance Monitor)

Track FPS/frame time with `stats.js` (from [mrdoob/stats.js](https://github.com/mrdoob/stats.js)):

```js
var stats = new Stats()
stats.setMode(0) // 0 = FPS, 1 = ms (frame time)
stats.dom.style.position = 'absolute'
stats.dom.style.left = '0px'
stats.dom.style.top = '0px'
document.body.appendChild(stats.dom)

// Update in render loop
function animate() {
	requestAnimationFrame(animate)
	stats.begin() // Start measurement
	// Render/update scene
	renderer.render(scene, camera)
	stats.end() // End measurement
}
animate()
```

#### 2.15.2. Key Optimization Tips

1. **Reuse Objects**: Use `clone()` for duplicate meshes (avoids re-creating geometry/material).
2. **Dispose Unused Resources**: Call `geometry.dispose()`, `material.dispose()`, `texture.dispose()` when objects are removed.
3. **Prefer BufferGeometry**: More efficient than `Geometry` (typed arrays, less GC).
4. **Compress Textures**: Use compressed formats (WebP, basis Universal) and resize large images.
5. **Optimize Render Loop**: Minimize calculations in `requestAnimationFrame`.
6. **Optimize 3D Models**: Use `gltf-pipeline`/`Draco` to compress GLTF models (reduce file size/load time).
