# Raymarcher

This is a very simple raymarching algorithm, which requires a ray origin $O$ (`ro`), and a ray direction $D$ (`rd`) as input.
The output of the function is the distance from the origin to the intersection point.
This intersection point is calculated with $P = O + t\cdotD$.

```glsl
#define MIN_MARCH_DIST 0.001
#define MAX_MARCH_DIST 20.
#define MAX_MARCH_STEPS 60.
float march(in vec3 ro, in vec3 rd) {
    float t = 0., 
        i = 0.;
    for(i=0.; i < MAX_MARCH_STEPS; i++) {
        vec3 p = ro + t*rd;
        float d = map(p);
        if(d < MIN_MARCH_DIST)
            break;
        t += d;
        if(t > MAX_MARCH_DIST)
            break;
    }
    if(i >= MAX_MARCH_STEPS) {
        t = MAX_MARCH_DIST;
    }
    return t;
}
```

The algorithm has the following global parameters:

 * `MIN_MARCH_DISTANCE`: The minimum distance to the object to qualify as a 'hit'. Lower values will increase the detail, but slow down the algorithm.
 * `MAX_MARCH_DISTNACE`: The maximum distance that a ray is allowed to travel. Larger values will render the scene further, but slow down the algorithm.
 * `MAX_MARCH_STEP`: The maximum amount of steps the algorithm is allowed to take. Increase this if the detail between complex objects is poor. Higher values will slow down the algorithm.