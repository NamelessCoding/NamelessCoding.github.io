## My first post about my fractal renderer found on shadertoy:

#**Implementation details**:

Currently so far I have implemented the microfacet model with the Beckmann brdf as well as importance sampling, next event estimation for both sun light and area light types with multiple importance sampling using the power heuristics, some very basic energy conservation and the preetham sky model. I also have written code for the GGX brdf, however when implementing it with NEE+MIS, the final result doesn't converge to naive as it's slightly off, so it's still something I need to fix.

Essentially what I'm doing is trying to solve the rendering equation using monte carlo. Since the rendering equation is mostly an integral that doesn't have a closed form solution, because we can't possibly predict all lighting scenarios and all types of environments with different types of materials, I'm sampling rays that have random directions based on the BRDF(unless of course the BRDF is a dirac delta function which means that it would be perfectly specular, in which case the only possible direction is the perfectly reflected one), bouncing them around the scene until the ray hits a light source, in essence trying to find the path from the light source to the eye in reverse.
That would of course be the basic path tracing algorithm.. Now since I'm doing NEE+MIS, I'm shooting a shadow ray towards the light source at each point I hit, calculating how much contribution it will have that's weighted by the power heuristics. For area light I'm calculating the pdf based on solid angle measure(r^2/area*cosine) and for the sun light it's based on the area of a spherical cap: https://en.wikipedia.org/wiki/Spherical_cap. Because the sampling for the sun is based on the angular radius theta, I can set how "big/close" the sun will be and due to that it's completely necessary to do multiple importance sampling. Because for a surface with very low roughness, the probability of the ray towards the sun(with a bigger angular radius) to be equal or close to the reflected ray is négligeable and thus the brdf will most likely return values close to 0, which makes the convergence rate for NEE in those cases to be very slow. And that's where MIS comes in, to combine both NEE and the brdf sampling together Such that each technique only contributes in the parts where it excels And that's what the power heuristic does, it ensures that the current technique won't sample in an area where another technique is better at, which is defined through the pdf. The pdf itself in basic terms works as normalization for estimating the integral. It will return higher numbers in places that have a high probability of being chosen and smaller numbers in places that have a lower probability, by dividing by the pdf we're essentially saying that the contribution of samples that have a higher probability of being chosen should be lowered and the contribution of samples with lower probability should be raised.

For the basic energy conservation at first I started reading this presentation: https://blog.selfshadow.com/publications/s2017-shading-course/imageworks/s2017_pbs_imageworks_slides_v2.pdf
and I was able to precompute the irradiance table, however afterwhich I found a much simpler solution in a seemingly unexepected place:https://c0de517e.blogspot.com/2019/08/misunderstanding-multiscattering.html
There is of course the issue of me using beckmann, rather than ggx, but I found that the above solution works surprisingly well.

The preetham sky model is a straightforward implementation of this paper: https://timothykol.com/pub/sky.pdf

Recently I learned about tonemapping and why it should be applied after the sampling process is done and not during, that's because light itself should be linear and tonemapping is not a linear function. We would essentially mess with the linearity of light if we were to apply tonemapping during and that would result(while it might look better) in something that's not equal to the ground truth.

I do hope to one day create my own brdf, which for the distribution function it's not really hard, however to have a complete brdf, I need to have a pdf and a sampling method. To get theta(for the sampling) I need to calculate the cdf by integrating the pdf and finally solving the integral. Not to mention that my brdf will need to be reciprocal and energy conserving, so due to my inexperience I'm still not completely able to do this yet. However I'm motivated and I'm learning more and more, so that I'd hopefully one day would be able to create a proper brdf that could be used in professional renderers. 

But enough about that, here are some pictures from renderer:

![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/weirdweird.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/sdfshkj12413523.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/pyramidsf.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/isitgood.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/hmmmi2.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/gsfdsfds.png?raw=true)



#Links to the shaders:
https://www.shadertoy.com/view/NtsGDl
https://www.shadertoy.com/view/sls3Ds
https://www.shadertoy.com/view/st2Gzh

[back](./)
