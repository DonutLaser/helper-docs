# GENERAL

## Camera

Camera is a virtual object. It does not exist in the world, but when we change rotation or position of the camera, it produces a transformation matrix that we apply to each rendered object at the time of rendering.
So, if we move the camera, transformation matrix changes and thus the rendered objects move. Camera lives in screen space and mouse position is in the screen space. Objects, on the other hand, live in the world space. If we want to translate screen space point to world space point, we need to take the point and apply an inverse of the transformation matrix to it which produces a point in world space. The opposite is possible, we just have to apply the transformation matrix to the point in world space, which is exactly what happens when you apply the transformation matrix to the rendered object.

To move objects on the screen properly when the camera moves, we need to apply an opposite translation. Since camera is not a real object, when we move the camera, in reality the objects on the screen move in the opposite direction rather than the camera itself. For example, we move the camera up. The object in the center should now be on the lower part of the screen. In order to achieve that we say that the translation is actually in the opposite direction (down) and when we apply that translation, the objects will move down and it will look like the camera just moved up.

To calculate transformation matrix, we want to do the following: `translationMatrix * rotationMatrix * scaleMatrix`. IMPORTANT: the first transformation is the scale here. The transformation order is right to left.

Camera is needed when the game's world expands beyond the viewport dimensions. If the world is the same size as viewport (or smaller), camera is not needed at all.

## Maths

### Transformation matrix

All of these work around the origin.

Identity matrix (no change):

```json
[1 0 0]
[0 1 0]
[0 0 1]
```

Translation matrix:

```json
[1 0 X]
[0 1 Y]
[0 0 1]
```

Scale matrix:

```json
[W 0 0]
[0 H 0]
[0 0 1]
```

Rotate matrix (clockwise):

```json
[ cos(a)  sin(a) 0]
[-sin(a)  cos(a) 0]
[   0       0    1]
```

Shear (skew) in x direction matrix:

```json
[1 X 0]
[0 1 0]
[0 0 1]
```

Shear (skew) in y direction matrix:

```json
[1 0 0]
[Y 1 0]
[0 0 1]
```

Reflect on both axes matrix:

```json
[-1  0 0]
[ 0 -1 0]
[ 0  0 1]
```

Reflect on x axis matrix:

```json
[1  0 0]
[0 -1 0]
[0  0 1]
```

Reflect on y axis matrix:

```json
[-1 0 0]
[ 0 1 0]
[ 0 0 1]
```

# UNITY

## Surface shader

#### Default surface shader:

```
Shader "Custom/NewSurfaceShader"
{
    Properties
    {
        _Color ("Color", Color) = (1,1,1,1)
        _MainTex ("Albedo (RGB)", 2D) = "white" {}
        _Glossiness ("Smoothness", Range(0,1)) = 0.5
        _Metallic ("Metallic", Range(0,1)) = 0.0
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 200

        CGPROGRAM
        // Physically based Standard lighting model, and enable shadows on all light types
        #pragma surface surf Standard fullforwardshadows

        // Use shader model 3.0 target, to get nicer looking lighting
        #pragma target 3.0

        sampler2D _MainTex;

        struct Input
        {
            float2 uv_MainTex;
        };

        half _Glossiness;
        half _Metallic;
        fixed4 _Color;

        // Add instancing support for this shader. You need to check 'Enable Instancing' on materials that use the shader.
        // See https://docs.unity3d.com/Manual/GPUInstancing.html for more information about instancing.
        // #pragma instancing_options assumeuniformscaling
        UNITY_INSTANCING_BUFFER_START(Props)
            // put more per-instance properties here
        UNITY_INSTANCING_BUFFER_END(Props)

        void surf (Input IN, inout SurfaceOutputStandard o)
        {
            // Albedo comes from a texture tinted by color
            fixed4 c = tex2D (_MainTex, IN.uv_MainTex) * _Color;
            o.Albedo = c.rgb;
            // Metallic and smoothness come from slider variables
            o.Metallic = _Metallic;
            o.Smoothness = _Glossiness;
            o.Alpha = c.a;
        }
        ENDCG
    }
    FallBack "Diffuse"
}
```

Explanation line by line:  
`Shader "Custom/NewSurfaceShader"`: shader starts with keyword `Shader`, the name describes where in the editor it will be shown when selecting the shader  
`Properties`: this is a properties block, these properties are the interface to the shader and will be shown in the editor  
`SubShader`: this is the shader description  
`Tags { "RenderType"="Opaque" }`: here you can describe the properties of the shader. In this case, we say that the shader is not transparent. For all available tags, see [https://docs.unity3d.com/Manual/SL-SubShaderTags.html](https://docs.unity3d.com/Manual/SL-SubShaderTags.html)
`CGPROGRAM / ENDCG`: these indicate the actual meat of the shader, up until this point, everything was written in ShaderLab, now everything will be written in Cg/HLSL  
`#pragma surface surf Standard fullforwardshadows`: pragma indicates that the surface function is `surf` and used the Standard lighting model with full shadows. It is possible to change lighting model and even write a custom one. For more, see [https://docs.unity3d.com/Manual/SL-SurfaceShaderLighting.html](https://docs.unity3d.com/Manual/SL-SurfaceShaderLighting.html)  
`#pragma target 3.0`: this indicates that we should use shader model 3.0  
`sampler2D _MainTex`: to use the property `_MainTex`, we create a variable `_MainTex` of type sampler2D. For the type explanation, see [https://docs.unity3d.com/Manual/SL-DataTypesAndPrecision.html](https://docs.unity3d.com/Manual/SL-DataTypesAndPrecision.html)  
`struct Input`: every surface shader has to have an Input structure, here we can specify what properties should Unity send to shader as input. For all available options, see [https://docs.unity3d.com/Manual/SL-SurfaceShaders.html](https://docs.unity3d.com/Manual/SL-SurfaceShaders.html)  
`void surf (Input IN, inout SurfaceOutputStandard 0)`: the definition of `surf` function which is where the main workings of the shader are specified

#### SurfaceOuput struct:

Surface output has several properties which can be used to determine the final aspect of the material:

-   `fixed3 Albedo`: the base colour / texture of an object,
-   `fixed3 Normal`: the direction of the face, which determines its reflection angle,
-   `fixed3 Emission`: how much light this object is generating by itself,
-   `half Specular`: how well the material reflects lights from 0 to 1,
-   `fixed Gloss`: how diffuse the specular reflection is,
-   `fixed Alpha`: how transparent the material is.

#### Built-in functions:

`tex2D`: function that gets the color from the texture based on the UVs. Signature: `(texture, uv)`

#### Custom lighting functions

You are supposed to multiply final color with shadow attenuation to render the received shadows

#### vert function

To add a vertex function to surface shader, write `#pramga vertex:vert`. Then specify the vertex function

```
void vert (inout appdata_full v) {
	// code here
}
```

`appdata_full` is a struct which contains all the data of the current vertex.

#### General knowledge

It is not recommended to write if..else statements in shaders as they decrease performance.
