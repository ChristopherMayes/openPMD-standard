Accelerator Lattice Extension for the openPMD Standard
==========================================

- This extension to the openPMD standard is indicated using:
  - `openPMDextension = Accelerator`


Definitions
-----------

  - **Lattice**: A **lattice** is the arrangement of elements in a particle accelerator machine. The
  word **lattice** can also be applied to other machines as well.

  - **Global** coordinate system: The **global** coordinate system is a coordinate system that is
  independent of the the **lattice**.

  - **Lattice** coordinate system: The curvilinear coordinate system whose "longitudinal"
  coordinate (typically called **s**) typically runs through the nominal centers of the elements in the machine. 



Notes:
------

- The `position` coordinates are **(x, y, s)** where **s** is the lattice longitudinal coordinate.


Additional Root Group (path `/`) Attributes
-------------------------

- `latticeName`
  - Type: Optional *(string)*
  - Description: The name of the lattice.

- `latticeFile`
  - Type: Optional *(string)*
  - Description: The name of the root lattice file.


Additional Particle Root Records
---------------------

- `latticeElementName`
  - Type: Optional *(string)*
  - Description: The name of the lattice element the particle is in.

- `latticeElementID`
  - Type Optional *(string)*
  - Description: The ID string for the lattice element the particle is in.
  - Example: With Bmad based programs the ID string is of the form 
    **branch-index>>element-index** where **branch-index** is the associated
branch index integer, and **element-index** is the associated lattice element index within the branch.

- `locationInElement`
  - Type Optional *(int)*
  - Description: String indicating where the particle is with respect to the lattice element. Possible values are:
    - `-1`: Upstream end of element outside of any edge fields.
    - `0`: Inside the element.
    - `1`: Downstream end of the element outside of any edge fields.
  - Rational: Since some programs will model edge fields of a lattice element as having zero length, the longitudinal **s**-position
does not necessarily provide enough information to determine where a particle is with respect to an edge field.

- `Time-RefTime`
  - Type: Optional *(float)*
  - Description: The particle time with respect to the time of the reference particle. 
Note that the reference time is a function of **s** and therefore can be different for different particles.


- `momentum`: 
  - Type: Optional *(float)*
  - Description: The total momentum of the particles relative to the `momentumOrigin` attribute.
    - `momentumOrigin`: 
      - Type: Optional *(float)* attribute
      - Description: Specifies the origin from with the momentum is measured with respect to.

- `Energy`:
  - Type: Optional *(float)*
  - Description: The total energy of the particles relative to the `energyOrigin` attribute.
    - `energyOrigin`:  
      - Type: Optional *(float)* attribute
      - Description: Specifies the origin from with the energy is measured with respect to.


- `latticeToGlobalTransformation/`
  - Type: Optional group.
  - Description: Defines the transformation from lattice coordinates to global coordinates at some
  particlar longitudinal **s**-position. Inclusion of this record only makes sense if all the
  particles are at the same **s**-position.
  - `R_frame`: 
    - Required 3-vector *(float)* Attribute
    - Description: pecifying the (x, y, z) position of the lattice coordinates with respect
  to the global coordinates.
  - `W_matrix`: 
    - Required 3 x 3 matrix *(float)* 
    - Description: Dataset holding the 3x3 transformation matrix from lattice coordinates to global
  coordinates.
  - Position Transformation: Position_global = W_matrix * Position_lattice + R_frame
  - Momentum transformation: Momentum_global = W_matrix * Momentum_lattice

- `phaseSpaceFirstOrderMoment`:
  - Type: Optional 6-vector *(float)*
  - Description: First order beam moments.

- `phaseSpaceSecondOrderMoment`:
  - Type: Optional 6x6-matrix *(float)*
  - Description: Second order beam moments.

- `phaseSpaceThirdOrderMoment`:
  - Type: Optional 6x6x6-tensor *(float)*
  - Description: Third order beam moments.


Additional Dataset Attributes
----------------------------- 

The following attributes can be used with any dataset:

- `minValue`:
  - Type: Optional *(float)*
  - Description: Minimum of the data.

- `maxValue`:
  - Type: Optional *(float)*
  - Description: Maximum of the data.
