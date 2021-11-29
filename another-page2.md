## My first post about my volumetric renderer found on shadertoy:

#**Implementation details**:

For these volumetric renders, I used the fractal as the density, meaning that if the current ray position is within a certain distance of the fractal,
the density will be set at 0.9. While if the distance is larger, density at that point will simply be 0. I'm doing the most basic algorithm to render volumes!
Essentially what I'm doing is marching forward at a fixed step size and at each point that the density is above 0, I'm shooting a ray, accumulating the density along the path, towards the sun. Finally I use that value, alongside with the density at the current point and the transmission value to find the energy at that point:
The entire thing in code looks like this:

```glsl
  float density = fbm(pos);
  if(density > 0.01){
    float densityAlongSunPath = 0.;
    vec3 pos2 = pos;
    for(int i = 0; i < 20; i++){
      densityAlongSunPath += fbm(pos2);
      pos2+=sunDirection*stepSize;
    }
  transmission *= 1.0-density;
  energy += exp(-vec3(0.5,1.,2.)*densityAlongSunPath*densityMultiplier)*density*transmission;
}
....

return energy;
```


![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/dfgd345346.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/index.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/sdf2345.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/cloudsss1.png?raw=true)


#Links to the shaders:
[Link]https://www.shadertoy.com/view/sdXXDM
[Link]https://www.shadertoy.com/view/NssXD7
[Link]https://www.shadertoy.com/view/ssXGDX

[back](./)
