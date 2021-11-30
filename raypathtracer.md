## 2D Ray/Path Tracing

# **Implementation details**:
Creating a 2D path/ray tracer isn't very difficult, however to make it more interesting I tried a trick to make it seem like it's spectral,
but in reality I'm only doing a different refraction value depending if I'm calculating the red, green, or blue channel. This gives the illussion of 
dispersion and it's fairly simple to implement.

# **2D Path Tracing**
When path tracing in the second dimension, you start off from the current pixel position, choose a random direction and you shoot the ray until it hits an object.
In this case the objects were either refractive, or were emissive. If the ray hits an emissive object it stops at that point and the throughput * light emission is added to the final color, overall very simple.One issue I've found that I've yet to figure out is that whenever I create a 2D prism, I never manage to get good 
dispersion, the way I would've expected it. There is a way to make it work and we'll talk about that after this paragraph is over. This method does however still
generate beautiful images as seen below:

![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/2pd5.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/2pd4.png?raw=true)

One big issue with this way of generating an image is that the noise can become a big hurdle to overcome, there are some denoisers out there,
one which I prefer being by Intel and Nvidia. However on the other hand, it takes a while for it to converge.

# **2D Ray Tracing**

Now what I mean by 2D "Ray Tracing" as opposed to 2D "Path Tracing" is that in this version I directly shoot the ray from the pixel position
towards the light source. THe same rules still apply, if it hits a refractive object, the light refracts, if it hits a light source, we add to
the final color. What this version differentiates from "Path Tracing" is that there is no noise and proper dispersion is more possible. 
Also I'm still doing the different refraction values per color channel!

![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/2dp.png?raw=true)
![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/2dp2.png?raw=true)

# **Conclusion**
Both methods yield awesome results, but in the end it really depends on you to chose which one you prefer. One thing for sure is that I feel like
2D games can really be pushed forward with 2D ray/path tracing!



[back](./)
