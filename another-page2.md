## My second post about my volumetric renderer found on shadertoy:
#Implementation details:

I'm doing the most basic algorithm for calculating volumes with raymarching.
Basically I shoot a ray and move it by a fixed step amount, usually in the beginning it's larger steps,
until I hit a volume after which I switch to a smaller step size. At each point of the cloud I shoot
a ray towards the sun accumulating the density along that path (I also linearly increase the step size each
iteration so that with fewer samples I can still catch shadows from nearby clouds), then I use beer's law(e^-dens)
with the accumulated density multiplied with the current density and the transmission variable. 
Essentially the bigger the accumulated density is, the less contribution it will have, the darker the clouds will appear.


![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/dfgd345346.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/index.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/sdf2345.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/cloudsss1.png?raw=true)


#Links to the shaders:
[Link]https://www.shadertoy.com/view/sdXXDM
[Link]https://www.shadertoy.com/view/NssXD7
[Link]https://www.shadertoy.com/view/ssXGDX

[back](./)
