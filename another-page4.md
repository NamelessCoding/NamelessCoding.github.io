## Fractal path tracing #2

#**Implementation details**:
So with this one I'm doing the BRDFs a bit different, I heard from someone that lerping the brdfs together is a proper way to combine multiple BRDFs together and
it's supposedly also energy conserving, which is very important. 

#**New sky and clouds using rayleigh and mie scattering**:
So I recently tried a more physically realistic model of the sky, as opposed to the preetham sky algorithm I implemented a while ago. This time I'm using
rayleigh scattering for the sky and the formula is taken from here https://en.wikipedia.org/wiki/Mie_scattering, which is surprising since this is an article
about Mie scattering and it has rayleigh approximation formula. Either way, for the clouds I applied some mie scattering and after messing around a bit, I managed
to create some good looking clouds and sky:
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/dfgdfh345346.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/sdfsfg345356.png?raw=true)

#**Water rendering and beer's law**:
For the water rendering I'm doing two things, one is to set the material as mostly reflective, so that the ray will bounce off it and add reflections and I'm 
also refracting the ray and marching it against a second SDF without the water and only the fractal. I'm recalculating the brdf for the new point and also
multiplying the values by beer's law: 
```glsl
exp(-depth*depthMultiplier)
```
I'm doing this to emulate out-scattering and absorption in the participating media.
Results can be seen below:


![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/243423fftt.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/hfghfj456457.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/sdasfasf234235.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/xcvxbdf435346345.png?raw=true)

[back](./)
