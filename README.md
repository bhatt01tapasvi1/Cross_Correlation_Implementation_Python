## Cross-Correlation Algorithm for PIV

### Problem Definition

The objective of Particle Image Velocimetry (PIV) is to measure the velocity field of a fluid flow by analyzing two consecutive images of tracer particles seeded in the flow. The algorithm computes the displacement vectors between corresponding interrogation windows in the two images, which represent the movement of the fluid over a known time interval.

### Assumptions

1. **Laminar Flow:** The flow is assumed to be smooth and orderly, with minimal turbulence.
2. **Tracer Particles:** The particles used for seeding the flow are small enough to accurately follow the flow without disturbing it.
3. **Uniform Illumination:** The lighting conditions are uniform across the images, ensuring consistent particle visibility.
4. **Negligible Brownian Motion:** The effect of Brownian motion on the particle displacement is considered negligible.

### Mathematical Description

The core mathematical operations involved in the cross-correlation algorithm are as follows:

#### 1. Cross-Correlation Function:

The cross-correlation function $$ R_{ij} $$ is used to determine the similarity between two interrogation windows from the first image $$ I_1 $$ and the second image $$ I_2 $$. The function is given by:

$$
R_{ij} = \sum_{m=-M}^{M} \sum_{n=-N}^{N} \left( I_1(x+m, y+n) - \overline{I_1} \right) \left( I_2(x+m+i, y+n+j) - \overline{I_2} \right)
$$

Where:
- $$ R_{ij} $$ is the cross-correlation function.
- $$ I_1(x+m, y+n) $$ and $$ I_2(x+m+i, y+n+j) $$ are the intensity values of the first and second images, respectively.
- $$ \overline{I_1} $$ and $$ \overline{I_2} $$ are the mean intensity values of the interrogation windows in images $$ I_1 $$ and $$ I_2 $$.
- $$ M $$ and $$ N $$ define the size of the interrogation window.

#### 2. Displacement Vector Determination:

The displacement vector $$ (\Delta x, \Delta y) $$ is found by identifying the peak of the cross-correlation function $$ R_{ij} $$, which indicates the most probable displacement between the two interrogation windows.

#### 3. Subpixel Resolution Adjustment:

To achieve higher accuracy, the displacement can be refined to subpixel resolution using a quadratic interpolation method. The subpixel displacement $$ x_{sub} $$ in the $$ x $$-direction is computed as:

$$
x_{sub} = \frac{I(x-1) - I(x+1)}{2 \left( I(x-1) + I(x+1) - 2I(x) \right)}
$$

Similarly, the subpixel displacement in the $$ y $$-direction can be calculated.

#### 4. Velocity Field Computation:

Finally, the velocity vector $$ \vec{V} $$ is computed from the displacement vector using the time interval $$ \Delta t $$ between the two images:

$$
\vec{V} = \frac{\Delta \vec{x}}{\Delta t}
$$

Where:
- $$ \vec{V} = (V_x, V_y) $$ is the velocity vector.
- $$ \Delta \vec{x} = (\Delta x, \Delta y) $$ is the displacement vector.
- $$ \Delta t $$ is the time interval between the two frames.

### Implementation in Python

This algorithm is implemented in Python using libraries such as NumPy, OpenCV, and Matplotlib. The detailed code implementation is provided in the repository, demonstrating how the mathematical concepts are translated into a working code.

### Conclusion

The PIV cross-correlation algorithm described here is a fundamental tool in fluid dynamics research, providing insight into flow patterns by analyzing the movement of particles between two consecutive frames. The mathematical foundations ensure that the algorithm accurately captures fluid motion, even at subpixel levels, allowing for precise velocity field computations.
