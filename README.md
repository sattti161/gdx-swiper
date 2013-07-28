# gdx-swiper

An example of a "Fruit Ninja" style swipe in LibGDX.

The tutorial can be seen here:  
https://github.com/mattdesl/lwjgl-basics/wiki/LibGDX-Finger-Swipe

Note that, since writing this, I have been developing a more efficient and more robust line rendering system. 
By using a shader, we only need one triangle strip per line. The shader also allows us to create the anti-aliasing
and stroke based on the line width (so its completely scalable). We do this by giving one edge of vertices a texcoord of
1.0, and the other edge a texcoord of -1.0. Then, in the fragment shader, we use abs() to ensure that the gradient goes from 0.0
(outer edges) to 1.0 (center of line). 

The smoothing is calculated a little like this:
```glsl
//the mirrored gradient in the range 0.0 to 1.0
float aa = 1.0 - abs(vTexCoord.y);

//the exact smooth amount
vec4 color = vColor; //vertex color
color.a *= smoothstep(0.0, smooth/thickness, aa);
gl_FragColor = color;
```

The technique can also be used to draw general lines, such as a circular progress bar:  
http://www.java-gaming.org/topics/circular-health-bar/29791/msg/274139/view.html#msg274139