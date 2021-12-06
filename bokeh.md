## Bokeh effect

The bokeh effect is fairly simple to implement, what we're trying to do is to model the aperature shape as well as the focal plane.

# **The code**
```glsl
        float focaldist = 1.3;
        float radius = 0.01;

        vec3 camdirection = vec3(0.,1.,0.);
        camdirection.yz = rot(camdirection.yz, rota);
        camdirection.xy = rot(camdirection.xy, rotb);
        vec3 sidex = vec3(1.,0.,0.);
        vec3 sidey = vec3(0.,0.,1.);
        sidex.xy = rot(sidex.xy,rotb);

        sidey.yz = rot(sidey.yz,rota);
        sidey.xy = rot(sidey.xy,rotb);
    
        float ang = rndf(r)*2.0*3.14159;
        float dist = min(length((vec2(0.0,-0.4)-uv)*0.2),1.);
        float scale = sqrt(rndf(r))*radius;
        vec2 offset = vec2(cos(ang), sin(ang))*scale;
      
    
       //NOT MY CODE////////////////////////
        vec3 focuspoint = p + ((d*focaldist) / dot(d,camdirection)); //these will lie on the focal plane
        /////////////////////////////////////
    

        p = p + sidex*offset.x;
        p = p + sidey*offset.y;

        d = normalize(focuspoint - p);
```

# **The aperature shape**
In the end "p" is the current ray position and "d" is the ray direction. I use a "rot" function, which basically rotates a 2D vector by a certain amount. I use it
to align the side vectors so that I can offset the ray position by a certain amount on the plane with a normal described by the "camdirection". By offsetting the ray 
position, we're emulating the aperature shape and because we're accumulating frames over time the shape of the aperature will become visible.  

# **The focal point**
The focus point is the point that will be the most clear, essentially the way it works is that the point on the focal plane will be the least offset as the ray direction is towards the point on the focal plane. Once it passes the focus point the rays will diverge more and more away from one another.

![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/external-content.duckduckgo.com.png?raw=true)

# **Conclusion**
Bokeh can be really beautiful if done correctly and you can do any shape for the bokeh, as long as you can generate random points within the shape.

Either way, here are some pictures generated using this method:

![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/dfsg.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/374c2d_9418ea138ccd46fab18bdc5d1cbbe9e5~mv2.webp?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/metalfra.png?raw=true)



[back](./)
