# 19 · GRAPHICS & SIGNAL PROCESSING

## 19.1 Graphics

### Rasterization

**Definition:** Converting vector geometry (triangles) into pixels on a screen.

**Pipeline stages:**
1. **Vertex processing:** Transform vertices from model → world → view → clip → screen space
2. **Primitive assembly:** Group vertices into triangles
3. **Rasterization:** Determine which pixels each triangle covers
4. **Fragment processing:** Compute color per pixel
5. **Output merging:** Blend with framebuffer

**Key concepts:**
- **Viewport transform:** Map NDC (−1 to 1) to screen coordinates
- **Clipping:** Discard geometry outside view frustum
- **Perspective divide:** Divide by w to get NDC
- **Z-buffer:** Depth test for visibility

**Why it matters:** Rasterization is the dominant real-time rendering method; GPUs are optimized for it.

**Recall:** *Geometry → image.*

### Ray Tracing

**Definition:** Simulate light by tracing rays from camera into scene.

**Algorithm:**
```
for each pixel:
    ray = camera_ray(x, y)
    color = trace(ray, depth=0)
    framebuffer[x, y] = color

trace(ray, depth):
    hit = closest_intersection(ray, scene)
    if no hit: return background
    if depth > max_depth: return ambient
    color = local_shading(hit)
    if reflective: color += reflect(ray, hit) × trace(reflected_ray, depth+1)
    if refractive: color += refract(ray, hit) × trace(refracted_ray, depth+1)
    return color
```

**Types:**
- **Whitted:** Recursive; reflections, refractions
- **Path tracing:** Monte Carlo; global illumination
- **Photon mapping:** Two-pass; caustics

**Advantages:** Physically accurate; handles reflections, shadows, GI.
**Disadvantages:** Computationally expensive.

**Hardware:** NVIDIA RTX; dedicated ray tracing cores.

**Recall:** *Simulate light rays.*

### Shading

**Definition:** Compute color of surface at a point.

**Models:**

**Phong:**
```
I = ambient + diffuse + specular
I = k_a × I_a + k_d × (N·L) × I_d + k_s × (R·V)^n × I_s
```
- Ambient: constant
- Diffuse: Lambertian; N·L
- Specular: highlight; (R·V)^n

**Blinn-Phong:** Use halfway vector H = (L+V)/|L+V|; (N·H)^n.

**PBR (Physically Based Rendering):**
- **BRDF:** Bidirectional Reflectance Distribution Function
- **Microfacet theory:** Surface = microfacets
- **Cook-Torrance:** GGX distribution, Smith geometry, Fresnel

**Recall:** *Color surfaces.*

### Texturing

**Definition:** Map 2D images onto 3D surfaces.

**UV mapping:** Assign (u, v) coordinates to vertices.

**Filtering:**
- **Nearest neighbor:** Blocky; fast
- **Bilinear:** Smooth; 4 texels
- **Trilinear:** Mipmap + bilinear
- **Anisotropic:** Viewing angle correction

**Mipmaps:** Precomputed downsampled versions; avoid aliasing.

**Texture types:** Diffuse (albedo), normal, roughness, metallic, ambient occlusion, displacement.

**Recall:** *Wrap images on surfaces.*

### Lighting Models

**Local illumination:** Only direct light.

**Global illumination:** Direct + indirect (bounced light).
- **Ray tracing:** Path tracing, photon mapping
- **Radiosity:** Finite elements; diffuse only
- **Real-time GI:** Lumen (UE5), voxel GI, screen-space GI

**Light types:** Point, directional, spot, area.

**BRDF properties:** Energy conservation, reciprocity, Helmholtz.

**Recall:** *How light interacts.*

### Rendering Pipeline

**Stages:**
1. **Application:** Scene graph, culling
2. **Geometry:** Vertex shader, tessellation, geometry shader
3. **Rasterization:** Triangle setup, scan conversion
4. **Fragment:** Fragment shader, depth test, blending
5. **Output:** Framebuffer, display

**Programmable stages:** Vertex, tessellation, geometry, fragment, compute.

**Fixed stages:** Clipping, rasterization, blending.

**Recall:** *Stages of rendering.*

### GPU Programming

**Shaders:**
- **Vertex shader:** Per-vertex; transform
- **Fragment shader:** Per-pixel; color
- **Geometry shader:** Per-primitive; emit/modify
- **Compute shader:** General-purpose; parallel

**APIs:** OpenGL, Vulkan, DirectX, Metal, WebGPU.

**Languages:** GLSL, HLSL, MSL, WGSL, SPIR-V.

**Recall:** *Program the GPU.*

### Shaders

**Example (GLSL fragment shader):**
```glsl
#version 330 core
in vec2 uv;
out vec4 color;
uniform sampler2D tex;
void main() {
    color = texture(tex, uv);
}
```

**Recall:** *Per-vertex/fragment code.*

### Animation

**Keyframe:** Pose at specific time.
**Interpolation:** Linear, spline (Bezier, Catmull-Rom).
**Rigging:** Skeleton hierarchy.
**Skinning:** Bind mesh to skeleton; linear blend skinning.
**Physics-based:** Ragdoll, procedural.

**Recall:** *Motion over time.*

### Physics Simulation

**Rigid bodies:** Position, orientation, velocity, angular velocity.
**Collision detection:** Broad phase (AABB, BVH), narrow phase (GJK, SAT).
**Integration:** Euler, Verlet, RK4.
**Fluids:** SPH, grid-based.
**Cloth:** Mass-spring, position-based dynamics.

**Recall:** *Simulate physics.*

**SOTA:** Real-time ray tracing, neural rendering, Gaussian splatting, differentiable rendering.

---

## 19.2 Signal Processing

### Signals

**Definition:** Function of one or more variables carrying information.

**Types:**
- **Continuous:** x(t); defined for all t
- **Discrete:** x[n]; defined for integer n

**Sampling theorem (Nyquist-Shannon):** To reconstruct signal with max frequency f_max, sample at fs > 2 × f_max.

**Aliasing:** High frequencies appear as low frequencies when undersampled.

**Quantization:** Discretize amplitude; introduces noise.

**Recall:** *Continuous vs discrete; sampling theorem, aliasing.*

### Fourier Analysis

**Fourier series:** Periodic signal = sum of sinusoids.
```
x(t) = Σ c_n e^(j2πnf₀t)
```

**Fourier transform:** Non-periodic signal → continuous spectrum.
```
X(f) = ∫ x(t) e^(−j2πft) dt
```

**DFT:** Discrete Fourier Transform.
```
X[k] = Σ_{n=0}^{N−1} x[n] e^(−j2πkn/N)
```

**FFT:** Fast Fourier Transform; O(N log N).

**Properties:** Linearity, time shift, frequency shift, convolution ↔ multiplication.

**Recall:** *DFT, FFT, frequency domain.*

### Filtering

**Types:**
- **Low-pass:** Smooth; remove high frequencies
- **High-pass:** Edges; remove low frequencies
- **Band-pass:** Specific range
- **Band-stop:** Remove specific range

**FIR (Finite Impulse Response):** y[n] = Σ b_k x[n−k]; linear phase.
**IIR (Infinite Impulse Response):** Feedback; y[n] = Σ b_k x[n−k] − Σ a_k y[n−k].

**Design:** Window method, Parks-McClellan, Butterworth, Chebyshev.

**Recall:** *Low-pass, high-pass, band-pass; FIR, IIR.*

### Convolution

**Definition:** Combine two signals.
```
(f * g)(t) = ∫ f(τ) g(t−τ) dτ
```

**Discrete:**
```
(f * g)[n] = Σ f[k] g[n−k]
```

**Theorem:** Convolution in time = multiplication in frequency.

**Use:** Filtering, smoothing, feature extraction (CNNs).

**Recall:** *Time domain vs frequency domain.*

### Compression

**Lossless:**
- **Huffman:** Variable-length codes
- **LZ77/LZ78:** Dictionary-based
- **DEFLATE:** LZ77 + Huffman (ZIP, gzip, PNG)
- **FLAC:** Audio; lossless

**Lossy:**
- **JPEG:** DCT + quantization
- **MP3/AAC:** Psychoacoustic model
- **H.264/H.265/AV1:** Video; motion compensation

**Rate-distortion:** Trade-off quality vs bitrate.

**Recall:** *Lossless vs lossy; audio, image, video.*

### Modulation

**Analog:**
- **AM:** Amplitude modulation
- **FM:** Frequency modulation
- **PM:** Phase modulation

**Digital:**
- **ASK:** Amplitude shift keying
- **FSK:** Frequency shift keying
- **PSK:** Phase shift keying (BPSK, QPSK)
- **QAM:** Quadrature amplitude modulation (16-QAM, 64-QAM)

**Constellation diagram:** Visualize digital modulation.

**Recall:** *AM, FM, QAM; digital modulation.*

### Wavelets

**Definition:** Localized oscillations; multi-resolution analysis.

**Wavelet transform:** Decompose signal into frequency + time.

**Examples:** Haar, Daubechies, Morlet.

**Use:** Compression (JPEG 2000), denoising, feature extraction.

**Recall:** *Multi-resolution analysis.*

### Spectral Analysis

**Power Spectral Density (PSD):** Distribution of power over frequency.

**Spectrogram:** Time-frequency representation.

**Window functions:** Hann, Hamming, Blackman; reduce spectral leakage.

**Recall:** *Power spectral density, spectrograms.*

### Adaptive Filters

**LMS (Least Mean Squares):** Update weights to minimize error.
**RLS (Recursive Least Squares):** Faster convergence; more computation.

**Use:** Noise cancellation, channel equalization, echo cancellation.

**Recall:** *LMS, RLS.*

**SOTA:** Neural compression, learned codecs, wavelets, compressed sensing.

---
