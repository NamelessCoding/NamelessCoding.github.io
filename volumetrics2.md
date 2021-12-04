## Fractal path tracing volumetrics

Volumetrics are very easy to implement and I'll give an explanation on the algorithm as well as I'll include some code to go alongside it.

# **The idea**

**The idea itself is very simple:**
- Record the first hit position and the camera position.
- Once the path tracing is done, start moving along the vector from the camera to the hit position over at certain intervals of distance and shoot a ray towards the light source.
- If you hit the light source, you add 1 to some variable, otherwise you don't add anything.
- In the end, simply divide the accumulation variable by the amount of steps it took you to move from the camera to the hit position.

# **The code**
```glsl
  vec3 difv = (p-prevpos)/5.;
  float accum = 0.0f;
  for(int i = 0; i < 5; i++){
  vec3 currpos = prevpos + difv*float(i+1)*rndf(rseed);
    if(trac(currpos, dirToLightSource)){
       if(l > 0.01){ // We've hit the light source
          accum += 1.0;
       }
    }

  }
accum /= 5.0;
color += accum*vec3(1.,0.8,0.2);
```
# **Rasterization alternative**
So this method can be very easily done in rasterization, all you need to do is get the position on the triangle, the camera position, a couple of matricies 
and finally the shadow map:

```glsl
float volume(vec3 vec, vec3 view, mat4 ligpp, mat4 ligvv){
vec3 dir = vec / 60.;
float accum = 0.;
vec3 curr = view;
for(int i = 0; i < 60; i++){
vec4 a = ligpp*ligvv*vec4(curr, 1.);
vec3 projCoords = a.xyz/a.w;
projCoords = projCoords * 0.5 + 0.5;
float currentDepth = projCoords.z;
float closestDepth = texture(shadowMap, projCoords.xy).r;
//float closestDepth = 1.;
accum += (currentDepth < closestDepth)  ? 1.0 : 0.0;
curr += dir;
}
return (accum/60.);
}
```
You can see real-time examples on my youtube channel:
https://www.youtube.com/watch?v=65E9_aDgF5k


# **Conclusion**

This is perhaps the simplest method to do volumetrics, so simple that I was wondering if it's worth the articles.. 
Note that this works best when accumulated, as you can see the rndf(rseed) function returns a number between 0 and 1
and that introduces noise, but it also means that we can do less samples since we're path tracing and frames are accumulated, the end result is smooth.

Either way, here are some pictures generated using this method:

![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/gsfgsdfsg.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/notugly.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/volumetric2-min1.png?raw=true)



[back](./)
