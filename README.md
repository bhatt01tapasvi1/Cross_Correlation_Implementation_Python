## Cross-Correlation Algorithm for PIV

### Problem Definition

The objective of Particle Image Velocimetry (PIV) is to measure the velocity field of a fluid flow by analyzing two consecutive images of tracer particles seeded in the flow. The algorithm computes the displacement vectors between corresponding interrogation windows in the two images, which represent the movement of the fluid over a known time interval.

### Assumptions

1. **Laminar Flow:** The flow is assumed to be smooth and orderly, with minimal turbulence.

### Mathematical Description

The core mathematical operations include:

#### 1. Cross-Correlation Function:

$$
R_{ij} = \sum_{m=-M}^{M} \sum_{n=-N}^{N} (I_1(x+m, y+n) - \overline{I_1})(I_2(x+m+i, y+n+j) - \overline{I_2})
$$

Where:
- \( R_{ij} \) is the cross-correlation function.
- \( I_1 \
