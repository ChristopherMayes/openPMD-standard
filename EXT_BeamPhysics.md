Extension to the openPMD Standard for Describing Particle Beams and X-rays
==========================================

- This extension to the openPMD standard is indicated using:
  - `openPMDextension = BeamPhysics, SpeciesType`
  - The `SpeciesType` extension must be used when using this extension.

Definitions
-----------

- **Lattice**: A **lattice** is the arrangement of elements in a machine such as a particle accelerator.

- **Global** coordinate system: The **global** coordinate system is a coordinate system that is
used to describe the position and orientation of machine elements in space. That is, the **global**
coordinate system is fixed with respect to the building or room where the machine is placed independent of the
machine itself.

- **Lattice** coordinate system: The curvilinear coordinate system whose "longitudinal"
coordinate (typically called **s**) typically runs through the nominal centers of the elements 
in the machine. Typically, the **lattice** coordinate system is used to describe misalignments
of lattice elements with respect to the rest of the lattice.

- **group**: A **group** is a container structure containing
a set of zero or more **attributes**, a set of zero or more **groups** (which can be called
**sub-groups**), and a set of zero or more **datasets**. Note: In HDF5, these are also
called **groups**.

- **dataset**: A **dataset** is a structure that contains a set of zero or more **attributes** and a
data aray (which may be multidimensional).  Note: In HDF5, these are also
called **datasets**.

- **record**: A **record** is a **group** (without any sub-groups) or a **dataset** that contains data 
on a physical quantity like particle charge or electric field. There are two types of **records**:
  - **scalar records** hold scalar quantity
values (like particle charge). If all the particles have the same charge, the value of the charge
is stored as an attribute of the **record** and there is no associated data array. That is, the
**record** is a **group**. If the particles have differing charges, the values are stored in an array
of the **scalar record**. In this case the **scalar record** is a **dataset**.
  - **array records** hold a set of **datasets**. In this case the **record** is a **group**.
    - Example: A **record** named **E** for holding electric field values may have three datasets holding
the components of the field named **E/x**, **E/y**, and **E/z**.

- **attribute**: An **attribute** is a variable associated with a **group** along with a
value. Example: **snapshotPath** is a string variable associated with the root **/** group.



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

- `SpeciesType`
  - Type: Required *(string)*
  - Description: The name of the particle species. Species names must conform to the
  `SpeciesType` extension. 
  - Example: `electron`, `H2O`.

- `charge`
  - Type: Optional *(int)*
  - Description: The charge state of the particles. Not needed if the charge can be computed
  from knowledge of the `SpeciesType`.

- `latticeElementName`
  - Type: Optional *(string)*
  - Description: The name of the lattice element the particles are in. This only makes sense if all
  particles are in the same lattice element.

- `latticeElementID`
  - Type Optional *(string)*
  - Description: The ID string for the lattice element the particle is in.
  - Example: With [Bmad](https://www.classe.cornell.edu/bmad/) based programs the ID string is of the form 
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
