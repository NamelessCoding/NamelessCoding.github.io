## Art of a good lens flare

# **Implementation details**:

Creating a lens flare is quite easy actually, all you need is a bunch of 2D SDFs, that you can then plug into an exp(-sdf) function in order to get a certian shape!
Here's a video showing my lens flare off:
https://imgur.com/a/zEem70z

![Octocat](https://github.com/NamelessCoding/NamelessCoding.github.io/blob/main/assets/images/bhkgyt8.png?raw=true)


I'll post my code here for anyone wanting to take a look:

```glsl

float torus(vec2 p, vec2 s){
    vec2 mm = normalize(p);
    return length(mm*s.x - p) - s.y;
}

float box(vec2 p, vec2 s){
vec2 a = abs(p)-s;
return max(a.x,a.y);
}

float hex(vec2 p, float s){
float box1 = box(p, vec2(s));
vec2 pos = rot(p, 45.);
return min(box1, box(pos, vec2(s)));
}

vec3 lens(vec2 p, vec2 mouse){
p*=10.;

vec3 col = vec3(0.);

col += sin(texture(iChannel0, normalize(mouse-p)*0.7).x)*exp(-(length(mouse-p)-1.2)*1.);
col += exp(-(length(-mouse*0.5-p)-0.5)*3.);
for(int i = 0; i < 5; i++){
col += exp(-hex(-mouse*(0.2-float(i)*0.1)-p, 0.5-float(i)*0.1)*2.);
}

col += exp(-torus(mouse*0.2-p, vec2(2., 0.01))*2.);

vec3 col2 = vec3(1.)*exp(-torus(-mouse - p, vec2(6., 0.3))*20.);
col2 *= sin(texture(iChannel0, normalize(-mouse-p)*0.7).x)*exp(-(length(-mouse-p)-2.2)*1.);
col2 *= vec3(0.9,0.5,0.2);
col2 *= length(mouse-p)*0.04;

vec3 col3 = vec3(1.)*exp(-torus(-mouse*0.2 - p, vec2(10., 2.))*4.);
col3 *= sin(texture(iChannel0, normalize(-mouse-p)*0.7).x)*exp(-(length(-mouse-p)-2.2)*1.);
col3 *= vec3(0.9,0.5,0.2);
col3 *= length(mouse-p)*0.04;

col += col2;
col += col3;
col*=exp(-(length(mouse-p)-4.2)*0.2);
//col += exp(-(length(p)-4.));

col *= pal(length(mouse-p)*0.1, vec3(0.0,0.6,0.9));
col += exp(-(length(mouse-p)-0.6)*0.3)*vec3(0.9,0.7,0.2);

//vec2 pos2 = (mouse)-p;
//pos2 = rot(pos2, mouse.x*8.);
//col += exp(-length(pos2*vec2(1., 0.01)))*exp(-(length(mouse*0.2-p)-4.2)*0.6);

return col;
}

```

