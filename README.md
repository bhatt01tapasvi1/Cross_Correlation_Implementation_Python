## Cross-Correlation Algorithm for PIV

### Problem Definition

The objective of Particle Image Velocimetry (PIV) is to measure the velocity field of a fluid flow by analyzing two consecutive images of tracer particles seeded in the flow. The algorithm computes the displacement vectors between corresponding interrogation windows in the two images, which represent the movement of the fluid over a known time interval.

### Assumptions

1. **Laminar Flow:** The flow is assumed to be smooth and orderly, with minimal turbulence.
2. **Small Displacement:** The displacement of particles between frames is small enough to be captured within the search window.
3. **Uniform Illumination:** The images have consistent lighting, and particle intensities remain constant.
4. **Linear Particle Motion:** Particles move in straight lines between frames, without any significant acceleration.
5. **Gaussian Noise Distribution:** Any noise in the images follows a Gaussian distribution, making it suitable for filtering techniques used in sub-pixel resolution.

### Mathematical Formulation

#### Cross-Correlation Function

The cross-correlation between two interrogation windows \( c_1 \) and \( c_2 \) is computed using:

\[
R(i, j) = \sum_{m=1}^{M}\sum_{n=1}^{N} \left( c_1(m, n) - \mu_{c1} \right) \left( c_2(m+i, n+j) - \mu_{c2} \right)
\]

where:
- \( c_1(m, n) \) and \( c_2(m+i, n+j) \) are the pixel intensities of the interrogation windows in the two images.
- \( \mu_{c1} \) and \( \mu_{c2} \) are the mean values of \( c_1 \) and \( c_2 \), respectively.
- \( i \) and \( j \) represent the shift between the windows in the x and y directions.
- \( M \) and \( N \) are the dimensions of the interrogation windows.

#### Normalization

The cross-correlation is normalized to prevent biases due to varying illumination:

\[
R_{norm}(i, j) = \frac{R(i, j)}{\sqrt{\left(\sum_{m=1}^{M}\sum_{n=1}^{N} (c_1(m, n) - \mu_{c1})^2 \right) \cdot \left( \sum_{m=1}^{M}\sum_{n=1}^{N} (c_2(m+i, n+j) - \mu_{c2})^2 \right)}}
\]

This normalization ensures that \( R_{norm}(i, j) \) is between -1 and 1, where 1 represents a perfect match.

#### Peak Detection

The peak of the cross-correlation matrix \( R_{norm}(i, j) \) indicates the most likely displacement vector \( (u, v) \) for the particles:

\[
(u, v) = \text{argmax}_{i,j} \, R_{norm}(i, j)
\]

This peak represents the integer-pixel displacement. To achieve sub-pixel accuracy, a parabolic or Gaussian fit is applied around the peak to interpolate the exact displacement.

#### Sub-Pixel Refinement

The sub-pixel displacement correction can be mathematically described as:

\[
\delta u = \frac{R(i+1, j) - R(i-1, j)}{2(R(i+1, j) + R(i-1, j) - 2R(i, j))}
\]

\[
\delta v = \frac{R(i, j+1) - R(i, j-1)}{2(R(i, j+1) + R(i, j-1) - 2R(i, j))}
\]

The final sub-pixel displacement vector is then:

\[
(u_{final}, v_{final}) = (u + \delta u, v + \delta v)
\]

#### Vector Correction

Given the computed displacement vectors \( u(x, y) \) and \( v(x, y) \), a correction step is applied to refine the vectors:

- **Normalized Median Test:** Compute the normalized residuals:

\[
\epsilon_{u}(x, y) = \frac{|u(x, y) - \text{median}(u_{neighbors})|}{\text{median}(|u_{neighbors} - \text{median}(u_{neighbors})|) + 0.1}
\]

\[
\epsilon_{v}(x, y) = \frac{|v(x, y) - \text{median}(v_{neighbors})|}{\text{median}(|v_{neighbors} - \text{median}(v_{neighbors})|) + 0.1}
\]

The normalized fluctuation is:

\[
\epsilon(x, y) = \sqrt{\epsilon_{u}^2(x, y) + \epsilon_{v}^2(x, y)}
\]

If \( \epsilon(x, y) > 2.0 \) (threshold), the vector at \( (x, y) \) is replaced by the bilinear interpolation of its neighbors:

\[
u(x, y) = \frac{u(x+1, y) + u(x-1, y) + u(x, y+1) + u(x, y-1)}{4}
\]

\[
v(x, y) = \frac{v(x+1, y) + v(x-1, y) + v(x, y+1) + v(x, y-1)}{4}
\]

### Result Scaling

Finally, the displacement vectors are scaled to obtain velocity vectors:

\[
U(x, y) = \frac{u_{final}(x, y) \cdot L}{T}
\]

\[
V(x, y) = \frac{v_{final}(x, y) \cdot L}{T}
\]

where:
- \( L \) is the spatial scale (e.g., meters per pixel).
- \( T \) is the temporal scale (time between frames).

### Output

The output is a velocity field \( (U(x, y), V(x, y)) \) that represents the fluid motion between the two images, which can be visualized using quiver plots, streamlines, or other flow visualization techniques.
