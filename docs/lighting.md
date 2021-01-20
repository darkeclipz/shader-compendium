# Lighting models

## GGX

The GGX lighting model is derived in this paper: http://www.cs.cornell.edu/~srm/publications/EGSR07-btdf.pdf.

```glsl
float G1V(float dotNV, float k) {
    return 1.0 / (dotNV * (1.0 - k) + k);
}

float brdf_ggx(vec3 N, vec3 V, vec3 L, float roughness, float F0) {
    float alpha = roughness * roughness;
    vec3 H = normalize(V+L);
    float dotNL = clamp(dot(N,L), 0., 1.);
    float dotNV = clamp(dot(N,V), 0., 1.);
    float dotNH = clamp(dot(N,H), 0., 1.);
    float dotLH = clamp(dot(L,H), 0., 1.);
    float alphaSqr = alpha*alpha;
    float pi = 3.14159;
    float denom = dotNH * dotNH * (alphaSqr - 1.0) + 1.0;
    float D = alphaSqr / (pi * denom * denom);
    float dotLH5 = pow(1.0 - dotLH, 5.0);
    float F = F0 + (1.0 - F0) * dotLH5;
    float k = alpha / 2.0;
    float vis = G1V(dotNL, k) * G1V(dotNV, k);
    return dotNL * D * F * vis;
}
```

Source: http://filmicworlds.com/blog/optimizing-ggx-shaders-with-dotlh/.