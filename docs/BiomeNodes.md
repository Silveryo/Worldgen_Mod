# HytaleServer Visual Node Editor - Complete Node Documentation

## Overview
This documentation covers ALL visual node editor nodes for biome/world generation in the HytaleServer codebase. These nodes are asset-based classes that define visual node properties via CODEC definitions and are used in the node-based biome/world generation editor.

**Total Node Categories**: 22
**Total Node Types Documented**: 100+

---

## Table of Contents
1. [Density Nodes](#density-nodes) - 63 nodes for terrain/height generation
2. [Curve Nodes](#curve-nodes) - 18 nodes for function remapping
3. [Pattern Nodes](#pattern-nodes) - 15 nodes for block matching
4. [Material Provider Nodes](#material-provider-nodes) - 13 nodes for block selection
5. [Position Provider Nodes](#position-provider-nodes) - 14 nodes for placement
6. [Prop Nodes](#prop-nodes) - 9 nodes for decoration placement
7. [Tint Provider Nodes](#tint-provider-nodes) - 3 nodes for coloration
8. [Vector Provider Nodes](#vector-provider-nodes) - 5 nodes for direction/offset
9. [Environment Provider Nodes](#environment-provider-nodes) - 2 nodes for environment settings
10. [Terrain Nodes](#terrain-nodes) - 2 nodes for terrain definitions
11. [Scanner Nodes](#scanner-nodes) - 5 nodes for position scanning
12. [Prop Assignment Nodes](#prop-assignment-nodes) - 5 nodes for prop distribution
13. [Point Generator Nodes](#point-generator-nodes) - 2 nodes for point generation
14. [Noise Generator Nodes](#noise-generator-nodes) - 3 nodes for noise generation
15. [Other Asset Nodes](#other-asset-nodes) - Block masks and material sets
16. [Sub-Node Categories](#sub-node-categories) - Helper nodes

---

## DENSITY NODES

**Purpose**: Density nodes generate scalar fields that determine terrain height, placement probability, and other spatial properties. They return double values at any 3D position.

### AbsDensityAsset
- **Node Name**: AbsDensityAsset
- **Purpose**: Returns absolute value of input density
- **Parameters**: None (implicit single input)
- **Child Assets**: Single DensityAsset input
- **Output**: |input|

### AmplitudeConstantAsset
- **Node Name**: AmplitudeConstantAsset
- **Purpose**: Applies constant amplitude to a density value
- **Parameters**:
  - Value (double): Constant amplitude multiplier
- **Child Assets**: None
- **Output**: value * constant

### AmplitudeDensityAsset
- **Node Name**: AmplitudeDensityAsset
- **Purpose**: Multiplies input density by amplitude from input
- **Parameters**: None (implicit)
- **Child Assets**: Single DensityAsset input
- **Output**: input * amplitude_density

### AnchorDensityAsset
- **Node Name**: AnchorDensityAsset
- **Purpose**: Anchors density to a specific reference point
- **Parameters**: Anchor parameters
- **Child Assets**: DensityAsset inputs

### AngleDensityAsset
- **Node Name**: AngleDensityAsset
- **Purpose**: Calculates angle-based density
- **Parameters**: Angle-related parameters
- **Child Assets**: DensityAsset inputs

### AxisDensityAsset
- **Node Name**: AxisDensityAsset
- **Purpose**: Projects density along a specific axis
- **Parameters**: None (implicit)
- **Child Assets**: Single DensityAsset input
- **Output**: density along specified axis

### BaseHeightDensityAsset
- **Node Name**: BaseHeightDensityAsset
- **Purpose**: Gets density from base height data layer
- **Parameters**:
  - BaseHeightName (String): Name of the height data layer to reference
  - Distance (Boolean): Whether to return distance to layer or actual height value
- **Child Assets**: None
- **Output**: Height or distance from base height layer

### CacheDensityAsset
- **Node Name**: CacheDensityAsset
- **Purpose**: Caches density values for performance optimization
- **Parameters**: None (implicit)
- **Child Assets**: Single DensityAsset input
- **Use Case**: Reuse expensive density computations multiple times

### CeilingDensityAsset
- **Node Name**: CeilingDensityAsset
- **Purpose**: Returns the ceiling (minimum) of input value and limit
- **Parameters**:
  - Limit (double): The maximum value to return
- **Child Assets**: Single DensityAsset input
- **Output**: min(input, limit)

### CellNoise2DDensityAsset
- **Node Name**: CellNoise2DDensityAsset
- **Purpose**: Generates 2D cellular/Worley noise for organic patterns
- **Parameters**:
  - ScaleX (double): X-axis scale (must be > 0.0)
  - ScaleZ (double): Z-axis scale (must be > 0.0)
  - Jitter (double): Randomness factor for cell centers
  - Octaves (int): Number of detail layers (must be > 0)
  - Seed (String): Seed key for reproducibility (default "A")
  - CellType (FastNoiseLite.CellularReturnType): Cell return type
    - CellValue: Returns cell center value
    - Distance: Returns distance to nearest cell center
    - Distance2: Returns squared distance
    - Distance2Add: Returns distance² + value
    - Distance2Sub: Returns distance² - value
    - Distance2Mul: Returns distance² * value
    - Distance2Div: Returns distance² / value
- **Child Assets**: None
- **Output**: Cellular noise value at position
- **Use Case**: Creating organic, cell-like terrain patterns

### CellNoise3DDensityAsset
- **Node Name**: CellNoise3DDensityAsset
- **Purpose**: Generates 3D cellular/Worley noise for volumetric patterns
- **Parameters**: Similar to CellNoise2DDensityAsset but with 3D Y-scale
- **Child Assets**: None
- **Output**: 3D cellular noise value

### CellWallDistanceDensityAsset
- **Node Name**: CellWallDistanceDensityAsset
- **Purpose**: Calculates distance to cell walls in cellular noise
- **Parameters**: Cell noise parameters
- **Child Assets**: None
- **Output**: Distance to nearest cell wall

### ClampDensityAsset
- **Node Name**: ClampDensityAsset
- **Purpose**: Clamps density value between two bounds
- **Parameters**:
  - WallA (double): Minimum bound (lower limit)
  - WallB (double): Maximum bound (upper limit)
- **Child Assets**: Single DensityAsset input
- **Output**: clamped value between WallA and WallB

### ConstantDensityAsset
- **Node Name**: ConstantDensityAsset
- **Purpose**: Returns a constant density value everywhere
- **Parameters**:
  - Value (double): The constant value to return
- **Child Assets**: None
- **Output**: Constant value
- **Use Case**: Flat terrain, placeholder, baseline

### CubeDensityAsset
- **Node Name**: CubeDensityAsset
- **Purpose**: Creates density based on cube shape
- **Parameters**:
  - Curve (CurveAsset): Optional curve for shaping cube edges
- **Child Assets**: Optional CurveAsset
- **Output**: Density inside cube (positive) and outside (negative)

### CuboidDensityAsset
- **Node Name**: CuboidDensityAsset
- **Purpose**: Creates density based on cuboid (rectangular prism) shape
- **Parameters**: Similar to CubeDensityAsset with separate axis dimensions
- **Child Assets**: Optional CurveAsset
- **Output**: Density inside cuboid and outside

### CurveMapperDensityAsset
- **Node Name**: CurveMapperDensityAsset
- **Purpose**: Maps input density through a curve function
- **Parameters**:
  - Curve (CurveAsset): The curve to apply to input values
- **Child Assets**: Single DensityAsset input, one CurveAsset
- **Output**: curve.apply(input_density)
- **Use Case**: Nonlinear transformation of density values

### CylinderDensityAsset
- **Node Name**: CylinderDensityAsset
- **Purpose**: Creates density based on cylinder shape
- **Parameters**:
  - RadialCurve (CurveAsset): Curve for radial distance from axis
  - AxialCurve (CurveAsset): Curve for axial distance along axis
  - NewYAxis (Vector3d): New Y-axis for cylinder orientation
  - Spin (double): Rotation angle around new Y-axis
- **Child Assets**: Two CurveAssets (radial and axial)
- **Output**: Density inside cylinder

### DistanceDensityAsset
- **Node Name**: DistanceDensityAsset
- **Purpose**: Returns density based on distance from origin
- **Parameters**:
  - Curve (CurveAsset): Optional curve for distance mapping
- **Child Assets**: Optional CurveAsset
- **Output**: Distance from origin (optionally curved)

### DistanceToBiomeEdgeDensityAsset
- **Node Name**: DistanceToBiomeEdgeDensityAsset
- **Purpose**: Calculates distance from current position to nearest biome edge
- **Parameters**: Biome-related parameters
- **Child Assets**: None
- **Output**: Distance to biome boundary
- **Use Case**: Creating biome transition zones

### EllipsoidDensityAsset
- **Node Name**: EllipsoidDensityAsset
- **Purpose**: Creates density based on ellipsoid shape
- **Parameters**:
  - Curve (CurveAsset): Density curve for shaping
  - Scale (Vector3d): X/Y/Z scale dimensions
  - NewYAxis (Vector3d): Orientation axis
  - Spin (double): Rotation angle
- **Child Assets**: CurveAsset
- **Output**: Density inside ellipsoid

### ExportedDensityAsset
- **Node Name**: ExportedDensityAsset
- **Purpose**: References an exported density node (reusable sub-node)
- **Parameters**: Reference identifier
- **Child Assets**: None
- **Use Case**: Creating reusable node graphs

### FastGradientWarpDensityAsset
- **Node Name**: FastGradientWarpDensityAsset
- **Purpose**: Warps density along gradient direction (optimized version)
- **Parameters**: Warp parameters
- **Child Assets**: DensityAsset inputs
- **Output**: Density warped along its own gradient

### FloorDensityAsset
- **Node Name**: FloorDensityAsset
- **Purpose**: Returns the floor (maximum) of input value and limit
- **Parameters**:
  - Limit (double): The minimum value to return
- **Child Assets**: Single DensityAsset input
- **Output**: max(input, limit)

### GradientDensityAsset
- **Node Name**: GradientDensityAsset
- **Purpose**: Calculates gradient angle of input density along an axis
- **Parameters**:
  - Axis (Vector3d): Direction axis to measure gradient (cannot be zero vector)
  - SampleRange (double): Distance to sample for gradient calculation (must be > 0.0)
- **Child Assets**: Single DensityAsset input
- **Output**: Gradient value along specified axis
- **Use Case**: Finding slopes, flow directions, steepness

### GradientWarpDensityAsset
- **Node Name**: GradientWarpDensityAsset
- **Purpose**: Warps density along gradient direction
- **Parameters**: Warp parameters
- **Child Assets**: DensityAsset inputs
- **Output**: Density warped along gradient

### ImportedDensityAsset
- **Node Name**: ImportedDensityAsset
- **Purpose**: References an imported density node from another file
- **Parameters**: Asset reference path
- **Child Assets**: None
- **Use Case**: Modularity and reusability across files

### InverterDensityAsset
- **Node Name**: InverterDensityAsset
- **Purpose**: Inverts (negates) input density value
- **Parameters**: None (implicit single input)
- **Child Assets**: Single DensityAsset input
- **Output**: -input

### MaxDensityAsset
- **Node Name**: MaxDensityAsset
- **Purpose**: Returns the maximum value from all input densities
- **Parameters**: None (implicit variable inputs)
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: max(input1, input2, ...)

### MinDensityAsset
- **Node Name**: MinDensityAsset
- **Purpose**: Returns the minimum value from all input densities
- **Parameters**: None (implicit variable inputs)
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: min(input1, input2, ...)

### MixDensityAsset
- **Node Name**: MixDensityAsset
- **Purpose**: Blends between two input densities based on influence density
- **Parameters**: None (implicit three inputs: densityA, densityB, influenceDensity)
- **Child Assets**: Three DensityAsset inputs
- **Output**: densityA * (1 - influence) + densityB * influence
- **Use Case**: Smooth biome transitions, layered materials

### MultiMixDensityAsset
- **Node Name**: MultiMixDensityAsset
- **Purpose**: Blends multiple densities with weights
- **Parameters**: Multiple density inputs and weights
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: Weighted blend of multiple inputs

### MultiplierDensityAsset
- **Node Name**: MultiplierDensityAsset
- **Purpose**: Multiplies all input density values together
- **Parameters**: None (implicit variable inputs)
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: input1 * input2 * input3 * ...

### NormalizerDensityAsset
- **Node Name**: NormalizerDensityAsset
- **Purpose**: Normalizes density values to a range
- **Parameters**: Normalization parameters (target range)
- **Child Assets**: DensityAsset input
- **Output**: Normalized density value
- **Use Case**: Bringing different density sources to same range

### OffsetConstantAsset
- **Node Name**: OffsetConstantAsset
- **Purpose**: Adds constant offset to density
- **Parameters**: Offset value
- **Child Assets**: None
- **Output**: input + constant

### OffsetDensityAsset
- **Node Name**: OffsetDensityAsset
- **Purpose**: Offsets density based on a Y function
- **Parameters**:
  - FunctionForY (NodeFunctionYOutAsset): Function to determine Y offset based on input
- **Child Assets**: Single DensityAsset input, NodeFunctionYOutAsset
- **Output**: Density offset by Y function value
- **Use Case**: Creating vertical layers with varying offsets

### PipelineDensityAsset
- **Node Name**: PipelineDensityAsset
- **Purpose**: Chains multiple density operations in sequence
- **Parameters**:
  - Pipeline (DensityAsset[]): Array of density nodes to execute in order
- **Child Assets**: Array of DensityAsset nodes
- **Output**: Result of last node in pipeline
- **Use Case**: Complex multi-step transformations

### PlaneDensityAsset
- **Node Name**: PlaneDensityAsset
- **Purpose**: Creates density based on plane shape
- **Parameters**: Plane parameters (normal, distance)
- **Child Assets**: CurveAsset
- **Output**: Density on one side of plane

### PositionsPinchDensityAsset
- **Node Name**: PositionsPinchDensityAsset
- **Purpose**: Pinches position space based on density
- **Parameters**: Pinch parameters
- **Child Assets**: DensityAsset inputs
- **Output**: Density evaluated at pinched coordinates

### PositionsTwistDensityAsset
- **Node Name**: PositionsTwistDensityAsset
- **Purpose**: Twists position space based on density
- **Parameters**: Twist parameters
- **Child Assets**: DensityAsset inputs
- **Output**: Density evaluated at twisted coordinates

### PowDensityAsset
- **Node Name**: PowDensityAsset
- **Purpose**: Raises input density to a power
- **Parameters**: Power value
- **Child Assets**: DensityAsset input
- **Output**: input^power

### RotatorDensityAsset
- **Node Name**: RotatorDensityAsset
- **Purpose**: Rotates density around an axis
- **Parameters**:
  - NewYAxis (Vector3d): Rotation axis
  - SpinAngle (double): Rotation angle in radians
- **Child Assets**: DensityAsset input
- **Output**: Density evaluated at rotated coordinates

### ScaleDensityAsset
- **Node Name**: ScaleDensityAsset
- **Purpose**: Scales input density along each axis
- **Parameters**:
  - ScaleX (double): X-axis scale (default 1.0)
  - ScaleY (double): Y-axis scale (default 1.0)
  - ScaleZ (double): Z-axis scale (default 1.0)
- **Child Assets**: Single DensityAsset input
- **Output**: Density evaluated at scaled coordinates
- **Use Case**: Stretching/shrinking terrain features

### SelectorDensityAsset
- **Node Name**: SelectorDensityAsset
- **Purpose**: Selects between densities based on condition
- **Parameters**: Selection condition
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: Selected density based on condition

### ShellDensityAsset
- **Node Name**: ShellDensityAsset
- **Purpose**: Creates hollow shell shape density
- **Parameters**:
  - Thickness (double): Shell wall thickness
  - Curve (CurveAsset): Optional shaping curve
- **Child Assets**: CurveAsset
- **Output**: Positive inside shell, negative outside

### SimplexNoise2dDensityAsset
- **Node Name**: SimplexNoise2dDensityAsset
- **Purpose**: Generates 2D simplex noise for natural terrain
- **Parameters**:
  - Lacunarity (double): Frequency multiplier per octave (must be > 0.0)
  - Persistence (double): Amplitude multiplier per octave (must be > 0.0)
  - Scale (double): Overall noise scale (must be > 0.0)
  - Octaves (int): Number of detail octaves (must be > 0)
  - Seed (String): Seed key (default "A")
- **Child Assets**: None
- **Output**: Simplex noise value at 2D position
- **Use Case**: Hills, mountains, natural terrain variation

### SimplexNoise3DDensityAsset
- **Node Name**: SimplexNoise3DDensityAsset
- **Purpose**: Generates 3D simplex noise for volumetric detail
- **Parameters**:
  - Lacunarity (double): Frequency multiplier per octave (must be > 0.0)
  - Persistence (double): Amplitude multiplier per octave (must be > 0.0)
  - ScaleXZ (double): Horizontal scale (must be > 0.0)
  - ScaleY (double): Vertical scale (must be > 0.0)
  - Octaves (int): Number of octaves (must be > 0)
  - Seed (String): Seed key
- **Child Assets**: None
- **Output**: Simplex noise value at 3D position
- **Use Case**: Caves, overhangs, 3D terrain features

### SliderDensityAsset
- **Node Name**: SliderDensityAsset
- **Purpose**: Slides density values within range
- **Parameters**: Slider parameters (min, max, slide amount)
- **Child Assets**: DensityAsset input
- **Output**: Sliding density values

### SmoothCeilingDensityAsset
- **Node Name**: SmoothCeilingDensityAsset
- **Purpose**: Smoothly clamps to minimum (smooth ceiling)
- **Parameters**:
  - Limit (double): Upper bound
  - Smoothness (double): Transition sharpness
- **Child Assets**: DensityAsset input
- **Output**: Smoothly clamped value below limit

### SmoothClampDensityAsset
- **Node Name**: SmoothClampDensityAsset
- **Purpose**: Smoothly clamps between two bounds
- **Parameters**:
  - WallA (double): Lower bound
  - WallB (double): Upper bound
  - Smoothness (double): Transition sharpness
- **Child Assets**: DensityAsset input
- **Output**: Smoothly clamped value between bounds

### SmoothFloorDensityAsset
- **Node Name**: SmoothFloorDensityAsset
- **Purpose**: Smoothly clamps to maximum (smooth floor)
- **Parameters**:
  - Limit (double): Lower bound
  - Smoothness (double): Transition sharpness
- **Child Assets**: DensityAsset input
- **Output**: Smoothly clamped value above limit

### SmoothMaxDensityAsset
- **Node Name**: SmoothMaxDensityAsset
- **Purpose**: Smoothly selects maximum from inputs
- **Parameters**:
  - Smoothness (double): Transition sharpness
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: Smoothly blended maximum

### SmoothMinDensityAsset
- **Node Name**: SmoothMinDensityAsset
- **Purpose**: Smoothly selects minimum from inputs
- **Parameters**:
  - Smoothness (double): Transition sharpness
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: Smoothly blended minimum

### SqrtDensityAsset
- **Node Name**: SqrtDensityAsset
- **Purpose**: Returns square root of input density
- **Parameters**: None (implicit single input)
- **Child Assets**: Single DensityAsset input
- **Output**: sqrt(input)

### SumDensityAsset
- **Node Name**: SumDensityAsset
- **Purpose**: Adds all input density values together
- **Parameters**: None (implicit variable inputs)
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: input1 + input2 + input3 + ...

### SwitchDensityAsset
- **Node Name**: SwitchDensityAsset
- **Purpose**: Switches between densities based on value
- **Parameters**: Switch value and options
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: Selected density based on switch value

### SwitchStateDensityAsset
- **Node Name**: SwitchStateDensityAsset
- **Purpose**: Switches based on state machine
- **Parameters**: State parameters
- **Child Assets**: Multiple DensityAsset inputs
- **Output**: Density based on current state

### TerrainDensityAsset
- **Node Name**: TerrainDensityAsset
- **Purpose**: Returns terrain density at position
- **Parameters**: None
- **Child Assets**: None
- **Output**: Terrain height density
- **Use Case**: Accessing actual terrain height in biome generation

### VectorWarpDensityAsset
- **Node Name**: VectorWarpDensityAsset
- **Purpose**: Warps density along a vector field
- **Parameters**: Vector provider parameters
- **Child Assets**: DensityAsset and VectorProviderAsset inputs
- **Output**: Density evaluated at warped position
- **Use Case**: Flowing terrain, directional displacement

### XOverrideDensityAsset
- **Node Name**: XOverrideDensityAsset
- **Purpose**: Overrides X coordinate in density calculation
- **Parameters**: X value or provider
- **Child Assets**: DensityAsset input
- **Output**: Density evaluated at (overrideX, y, z)

### XValueDensityAsset
- **Node Name**: XValueDensityAsset
- **Purpose**: Returns X coordinate value as density
- **Parameters**: None
- **Child Assets**: None
- **Output**: World X coordinate

### YOverrideDensityAsset
- **Node Name**: YOverrideDensityAsset
- **Purpose**: Overrides Y coordinate in density calculation
- **Parameters**: Y value or provider
- **Child Assets**: DensityAsset input
- **Output**: Density evaluated at (x, overrideY, z)

### YValueDensityAsset
- **Node Name**: YValueDensityAsset
- **Purpose**: Returns Y coordinate value as density
- **Parameters**: None
- **Child Assets**: None
- **Output**: World Y coordinate

### ZOverrideDensityAsset
- **Node Name**: ZOverrideDensityAsset
- **Purpose**: Overrides Z coordinate in density calculation
- **Parameters**: Z value or provider
- **Child Assets**: DensityAsset input
- **Output**: Density evaluated at (x, y, overrideZ)

### ZValueDensityAsset
- **Node Name**: ZValueDensityAsset
- **Purpose**: Returns Z coordinate value as density
- **Parameters**: None
- **Child Assets**: None
- **Output**: World Z coordinate

---

## CURVE NODES

**Purpose**: Curve nodes remap input values through mathematical functions. They take a double input and return a transformed double output.

### CeilingCurveAsset
- **Node Name**: CeilingCurveAsset
- **Purpose**: Returns ceiling (floor) of input value
- **Parameters**: None (implicit single input)
- **Child Assets**: None
- **Output**: ceil(input)

### ClampCurveAsset
- **Node Name**: ClampCurveAsset
- **Purpose**: Clamps value between min and max
- **Parameters**:
  - Min (double): Lower bound
  - Max (double): Upper bound
- **Child Assets**: None
- **Output**: clamped value

### ConstantCurveAsset
- **Node Name**: ConstantCurveAsset
- **Purpose**: Returns constant value regardless of input
- **Parameters**:
  - Value (double): Constant value to return
- **Child Assets**: None
- **Output**: Constant value

### DistanceExponentialCurveAsset
- **Node Name**: DistanceExponentialCurveAsset
- **Purpose**: Exponential distance falloff curve
- **Parameters**: Distance and exponent parameters
- **Child Assets**: None
- **Output**: exp(-distance * exponent)

### DistanceSCurveAsset
- **Node Name**: DistanceSCurveAsset
- **Purpose**: S-curve distance interpolation (smoothstep)
- **Parameters**: Distance parameters
- **Child Assets**: None
- **Output**: Smooth S-shaped interpolation

### FloorCurveAsset
- **Node Name**: FloorCurveAsset
- **Purpose**: Returns floor of input value
- **Parameters**: None (implicit single input)
- **Child Assets**: None
- **Output**: floor(input)

### ImportedCurveAsset
- **Node Name**: ImportedCurveAsset
- **Purpose**: References imported curve
- **Parameters**: Asset reference
- **Child Assets**: None

### InverterCurveAsset
- **Node Name**: InverterCurveAsset
- **Purpose**: Inverts input (1 - value)
- **Parameters**: None (implicit single input)
- **Child Assets**: None
- **Output**: 1.0 - input

### ManualCurveAsset
- **Node Name**: ManualCurveAsset
- **Purpose**: User-defined curve with control points
- **Parameters**:
  - Points (PointInOutAsset[]): Array of control points with input/output pairs
- **Child Assets**: Multiple PointInOutAsset nodes
- **Output**: Interpolated value from control points
- **Use Case**: Custom arbitrary functions

### MaxCurveAsset
- **Node Name**: MaxCurveAsset
- **Purpose**: Returns maximum of inputs
- **Parameters**: Multiple curve inputs
- **Child Assets**: Multiple CurveAsset inputs
- **Output**: max(input1, input2, ...)

### MinCurveAsset
- **Node Name**: MinCurveAsset
- **Purpose**: Returns minimum of inputs
- **Parameters**: Multiple curve inputs
- **Child Assets**: Multiple CurveAsset inputs
- **Output**: min(input1, input2, ...)

### MultiplierCurveAsset
- **Node Name**: MultiplierCurveAsset
- **Purpose**: Multiplies input values
- **Parameters**: Multiple curve inputs
- **Child Assets**: Multiple CurveAsset inputs
- **Output**: input1 * input2 * input3 * ...

### NotCurveAsset
- **Node Name**: NotCurveAsset
- **Purpose**: Boolean NOT operation (for boolean curves)
- **Parameters**: Boolean curve input
- **Child Assets**: Single CurveAsset input
- **Output**: !input (true/false)

### SmoothCeilingCurveAsset
- **Node Name**: SmoothCeilingCurveAsset
- **Purpose**: Smooth ceiling operation
- **Parameters**:
  - Limit (double): Upper bound
  - Smoothness (double): Transition smoothness
- **Child Assets**: None
- **Output**: Smoothly clamped value below limit

### SmoothClampCurveAsset
- **Node Name**: SmoothClampCurveAsset
- **Purpose**: Smooth clamp operation
- **Parameters**:
  - Min (double): Lower bound
  - Max (double): Upper bound
  - Smoothness (double): Transition smoothness
- **Child Assets**: None
- **Output**: Smoothly clamped value

### SmoothFloorCurveAsset
- **Node Name**: SmoothFloorCurveAsset
- **Purpose**: Smooth floor operation
- **Parameters**:
  - Limit (double): Lower bound
  - Smoothness (double): Transition smoothness
- **Child Assets**: None
- **Output**: Smoothly clamped value above limit

### SmoothMaxCurveAsset
- **Node Name**: SmoothMaxCurveAsset
- **Purpose**: Smooth maximum of inputs
- **Parameters**: Smoothness parameter
- **Child Assets**: Multiple CurveAsset inputs
- **Output**: Smoothly blended maximum

### SmoothMinCurveAsset
- **Node Name**: SmoothMinCurveAsset
- **Purpose**: Smooth minimum of inputs
- **Parameters**: Smoothness parameter
- **Child Assets**: Multiple CurveAsset inputs
- **Output**: Smoothly blended minimum

### SumCurveAsset
- **Node Name**: SumCurveAsset
- **Purpose**: Adds input values
- **Parameters**: Multiple curve inputs
- **Child Assets**: Multiple CurveAsset inputs
- **Output**: input1 + input2 + input3 + ...

---

## PATTERN NODES

**Purpose**: Pattern nodes determine block matching rules. They return boolean true/false indicating whether a position/block matches criteria.

### AndPatternAsset
- **Node Name**: AndPatternAsset
- **Purpose**: Returns true only if ALL child patterns match
- **Parameters**:
  - Patterns (PatternAsset[]): Array of pattern nodes to check
- **Child Assets**: Multiple PatternAsset nodes
- **Output**: pattern1 && pattern2 && pattern3 && ...

### BlockSetPatternAsset
- **Node Name**: BlockSetPatternAsset
- **Purpose**: Matches blocks from a defined block set
- **Parameters**: Block set definition
- **Child Assets**: BlockSetAsset
- **Output**: True if block is in set

### CeilingPatternAsset
- **Node Name**: CeilingPatternAsset
- **Purpose**: Matches ceiling (top) surfaces of structures
- **Parameters**: Pattern parameters
- **Child Assets**: PatternAsset
- **Output**: True if at ceiling position

### ConstantPatternAsset
- **Node Name**: ConstantPatternAsset
- **Purpose**: Always returns constant true/false value
- **Parameters**:
  - Value (boolean): Constant boolean value to return
- **Child Assets**: None
- **Output**: Constant true or false

### CuboidPatternAsset
- **Node Name**: CuboidPatternAsset
- **Purpose**: Matches positions within a cuboid region
- **Parameters**: Cuboid dimensions (min/max corners)
- **Child Assets**: None
- **Output**: True if inside cuboid

### DensityPatternAsset
- **Node Name**: DensityPatternAsset
- **Purpose**: Matches based on density value within delimiters
- **Parameters**:
  - Delimiters (DelimiterAsset[]): Array of min/max delimiter pairs
  - FieldFunction (DensityAsset): Density node to evaluate
- **Child Assets**: Multiple DelimiterAsset nodes, one DensityAsset node
  - DelimiterAsset: Contains Min (double) and Max (double)
- **Output**: True if density within any delimiter range
- **Use Case**: Zone-based block selection, elevation-based materials

### FloorPatternAsset
- **Node Name**: FloorPatternAsset
- **Purpose**: Matches floor (bottom) surfaces of structures
- **Parameters**: Pattern parameters
- **Child Assets**: PatternAsset
- **Output**: True if at floor position

### GapPatternAsset
- **Node Name**: GapPatternAsset
- **Purpose**: Matches gaps between blocks (air/solid boundaries)
- **Parameters**: Gap parameters
- **Child Assets**: None
- **Output**: True if in gap position

### ImportedPatternAsset
- **Node Name**: ImportedPatternAsset
- **Purpose**: References imported pattern
- **Parameters**: Asset reference
- **Child Assets**: None

### MaterialPatternAsset
- **Node Name**: MaterialPatternAsset
- **Purpose**: Matches specific material type
- **Parameters**:
  - Material (MaterialAsset): Material to match
- **Child Assets**: MaterialAsset
- **Output**: True if block matches material

### NotPatternAsset
- **Node Name**: NotPatternAsset
- **Purpose**: Inverts pattern result
- **Parameters**: None (implicit single input)
- **Child Assets**: Single PatternAsset input
- **Output**: !input_pattern

### OffsetPatternAsset
- **Node Name**: OffsetPatternAsset
- **Purpose**: Offsets pattern check position
- **Parameters**: Offset amount (x, y, z)
- **Child Assets**: PatternAsset
- **Output**: Pattern evaluated at offset position

### OrPatternAsset
- **Node Name**: OrPatternAsset
- **Purpose**: Returns true if ANY child pattern matches
- **Parameters**:
  - Patterns (PatternAsset[]): Array of pattern nodes to check
- **Child Assets**: Multiple PatternAsset nodes
- **Output**: pattern1 || pattern2 || pattern3 || ...

### SurfacePatternAsset
- **Node Name**: SurfacePatternAsset
- **Purpose**: Matches positions relative to surface/medium boundary
- **Parameters**:
  - Surface (PatternAsset): Pattern to match at surface
  - Medium (PatternAsset): Pattern for medium (filling interior)
  - SurfaceRadius (double): Distance from surface to check (>= 0.0)
  - MediumRadius (double): Distance into medium to check (>= 0.0)
  - SurfaceGap (int): Gap from surface (>= 0)
  - MediumGap (int): Gap into medium (>= 0)
  - RequireAllFacings (boolean): If true, all facings must match
  - Facings (SurfacePattern.Facing[]): Array of facings to check
    - Facing enum values: NORTH, SOUTH, EAST, WEST, UP, DOWN
- **Child Assets**: Two PatternAsset nodes
- **Output**: True if position matches surface criteria
- **Use Case**: Placing decorations on surfaces, hanging items, wall decorations

### WallPatternAsset
- **Node Name**: WallPatternAsset
- **Purpose**: Matches wall surfaces (sides of structures)
- **Parameters**: Wall parameters (which directions, thickness)
- **Child Assets**: PatternAsset
- **Output**: True if on wall surface

---

## MATERIAL PROVIDER NODES

**Purpose**: Material Provider nodes determine which block/material to place at a given position.

### ConstantMaterialProviderAsset
- **Node Name**: ConstantMaterialProviderAsset
- **Purpose**: Always returns the same material everywhere
- **Parameters**:
  - Material (MaterialAsset): The material to always return
- **Child Assets**: Single MaterialAsset node
- **Output**: Constant material

### DownwardDepthMaterialProviderAsset
- **Node Name**: DownwardDepthMaterialProviderAsset
- **Purpose**: Provides materials for downward depth (layers below)
- **Parameters**:
  - Depth (int): Number of blocks downward to provide material
  - Material (MaterialProviderAsset): Material provider for the region
- **Child Assets**: Single MaterialProviderAsset node
- **Output**: Material at specified downward depth

### DownwardSpaceMaterialProviderAsset
- **Node Name**: DownwardSpaceMaterialProviderAsset
- **Purpose**: Provides materials in downward space (continuous downward region)
- **Parameters**: Space parameters (depth, material)
- **Child Assets**: MaterialProviderAsset
- **Output**: Material for downward space

### FieldFunctionMaterialProviderAsset
- **Node Name**: FieldFunctionMaterialProviderAsset
- **Purpose**: Provides materials based on field function (density) values
- **Parameters**: Field function parameters with material mappings
- **Child Assets**: DensityAsset
- **Output**: Material based on density value

### ImportedMaterialProviderAsset
- **Node Name**: ImportedMaterialProviderAsset
- **Purpose**: References imported material provider
- **Parameters**: Asset reference
- **Child Assets**: None

### QueueMaterialProviderAsset
- **Node Name**: QueueMaterialProviderAsset
- **Purpose**: Queues multiple material providers in sequence
- **Parameters**: Material provider list
- **Child Assets**: Multiple MaterialProviderAsset nodes
- **Output**: Materials in queue order

### SimpleHorizontalMaterialProviderAsset
- **Node Name**: SimpleHorizontalMaterialProviderAsset
- **Purpose**: Provides materials horizontally
- **Parameters**: Horizontal parameters (width, materials)
- **Child Assets**: MaterialProviderAsset
- **Output**: Material based on horizontal position

### SolidityMaterialProviderAsset
- **Node Name**: SolidityMaterialProviderAsset
- **Purpose**: Provides materials based on solidity (solid vs non-solid)
- **Parameters**: Solidity check parameters with material mappings
- **Child Assets**: MaterialProviderAsset
- **Output**: Material based on solidity state

### StripedMaterialProviderAsset
- **Node Name**: StripedMaterialProviderAsset
- **Purpose**: Provides alternating materials in stripes
- **Parameters**:
  - Stripes (Stripe[]): Array of stripe definitions with materials and widths
- **Child Assets**: Multiple MaterialProviderAsset nodes
- **Output**: Material based on stripe pattern

### TerrainDensityMaterialProviderAsset
- **Node Name**: TerrainDensityMaterialProviderAsset
- **Purpose**: Provides materials based on terrain density
- **Parameters**: Terrain density reference with material mappings
- **Child Assets**: TerrainAsset
- **Output**: Material based on terrain density value

### UpwardDepthMaterialProviderAsset
- **Node Name**: UpwardDepthMaterialProviderAsset
- **Purpose**: Provides materials for upward depth (layers above)
- **Parameters**:
  - Depth (int): Number of blocks upward to provide material
  - Material (MaterialProviderAsset): Material provider for the region
- **Child Assets**: Single MaterialProviderAsset node
- **Output**: Material at specified upward depth

### UpwardSpaceMaterialProviderAsset
- **Node Name**: UpwardSpaceMaterialProviderAsset
- **Purpose**: Provides materials in upward space (continuous upward region)
- **Parameters**: Space parameters (depth, material)
- **Child Assets**: MaterialProviderAsset
- **Output**: Material for upward space

### WeightedMaterialProviderAsset
- **Node Name**: WeightedMaterialProviderAsset
- **Purpose**: Randomly selects material based on weights
- **Parameters**:
  - WeightedMaterials (WeightedMaterialAsset[]): Array of weighted materials
    - Each has: Weight (double, MaterialProviderAsset)
  - SkipChance (double): Probability of returning no material (0.0 to 1.0)
  - Seed (String): Seed key for random selection
- **Child Assets**: Multiple WeightedMaterialAsset nodes (each containing a MaterialProviderAsset)
- **Output**: Randomly selected material based on weights
- **Use Case**: Randomized terrain, variety distribution

---

## POSITION PROVIDER NODES

**Purpose**: Position Provider nodes determine where to place items (props, structures, decorations).

### AnchorPositionProviderAsset
- **Node Name**: AnchorPositionProviderAsset
- **Purpose**: Provides positions anchored to a reference point
- **Parameters**: Anchor parameters
- **Child Assets**: None
- **Output**: Positions relative to anchor point

### BaseHeightPositionProviderAsset
- **Node Name**: BaseHeightPositionProviderAsset
- **Purpose**: Provides positions based on base height data layer
- **Parameters**:
  - BaseHeightName (String): Name of height layer to reference
- **Child Assets**: None
- **Output**: Position at base height

### CachedPositionProviderAsset
- **Node Name**: CachedPositionProviderAsset
- **Purpose**: Caches position provider results for performance
- **Parameters**: Cache parameters
- **Child Assets**: PositionProviderAsset
- **Output**: Cached positions

### FieldFunctionOccurrencePositionProviderAsset
- **Node Name**: FieldFunctionOccurrencePositionProviderAsset
- **Purpose**: Provides positions based on field function occurrences
- **Parameters**: Field function and occurrence parameters
- **Child Assets**: DensityAsset
- **Output**: Positions where field function meets criteria

### FieldFunctionPositionProviderAsset
- **Node Name**: FieldFunctionPositionProviderAsset
- **Purpose**: Provides positions based on field function
- **Parameters**: Field function parameters
- **Child Assets**: DensityAsset
- **Output**: Positions derived from field function

### ImportedPositionProviderAsset
- **Node Name**: ImportedPositionProviderAsset
- **Purpose**: References imported position provider
- **Parameters**: Asset reference
- **Child Assets**: None

### ListPositionProviderAsset
- **Node Name**: ListPositionProviderAsset
- **Purpose**: Provides positions from a predefined list
- **Parameters**:
  - Positions (Vector3i[]): Array of position vectors
- **Child Assets**: None
- **Output**: Positions from list

### Mesh2DPositionProviderAsset
- **Node Name**: Mesh2DPositionProviderAsset
- **Purpose**: Provides positions in a 2D mesh grid pattern
- **Parameters**:
  - Spacing (int): Distance between mesh points
  - Bounds (Rectangle): X/Z bounds of mesh
- **Child Assets**: None
- **Output**: Grid positions in 2D

### Mesh3DPositionProviderAsset
- **Node Name**: Mesh3DPositionProviderAsset
- **Purpose**: Provides positions in a 3D mesh grid pattern
- **Parameters**:
  - Spacing (Vector3i): X/Y/Z spacing
  - Bounds (Cuboid): 3D bounds of mesh
- **Child Assets**: None
- **Output**: Grid positions in 3D

### OffsetPositionProviderAsset
- **Node Name**: OffsetPositionProviderAsset
- **Purpose**: Offsets positions by a vector
- **Parameters**:
  - Offset (Vector3d): X/Y/Z offset amount
- **Child Assets**: PositionProviderAsset
- **Output**: Input positions + offset

### SpherePositionProviderAsset
- **Node Name**: SpherePositionProviderAsset
- **Purpose**: Provides positions on sphere surface/volume
- **Parameters**:
  - Radius (double): Sphere radius
  - Center (Vector3d): Sphere center position
  - Type (Surface/Volume): Whether to place on surface or throughout volume
- **Child Assets**: None
- **Output**: Positions on/in sphere

### UnionPositionProviderAsset
- **Node Name**: UnionPositionProviderAsset
- **Purpose**: Combines multiple position providers
- **Parameters**:
  - Providers (PositionProviderAsset[]): Array of position providers
- **Child Assets**: Multiple PositionProviderAsset nodes
- **Output**: Combined positions from all providers

### VerticalEliminatorPositionProviderAsset
- **Node Name**: VerticalEliminatorPositionProviderAsset
- **Purpose**: Eliminates vertical positions (removes duplicates along Y-axis)
- **Parameters**: Elimination parameters
- **Child Assets**: PositionProviderAsset
- **Output**: Unique positions without vertical stacking

---

## PROP NODES

**Purpose**: Prop nodes determine how decorations (trees, rocks, flowers, structures) are placed in the world.

### BoxPropAsset
- **Node Name**: BoxPropAsset
- **Purpose**: Places props in a box pattern
- **Parameters**:
  - Dimensions (Vector3i): X/Y/Z size of box
  - Pattern (PatternAsset): Where props can be placed
  - Scanner (ScannerAsset): How to find positions
  - MaterialProvider (MaterialProviderAsset): Material to use
  - Density (DensityAsset): Placement density
  - BlockMask (BlockMaskAsset): Block restrictions
- **Child Assets**: PatternAsset, ScannerAsset, MaterialProviderAsset, DensityAsset, BlockMaskAsset
- **Output**: Props placed in box region

### ClusterPropAsset
- **Node Name**: ClusterPropAsset
- **Purpose**: Places props in organic clusters (random groups)
- **Parameters**:
  - Range (int): Maximum cluster radius (>= 0)
  - DistanceCurve (CurveAsset): Distance-based distribution curve
  - Seed (String): Seed key for random selection
  - WeightedProps (WeightedPropAsset[]): Array of weighted props to place
    - Each has: Weight (double, PropAsset)
  - Pattern (PatternAsset): Pattern for placement
  - Scanner (ScannerAsset): Scanner for finding positions
- **Child Assets**: Multiple WeightedPropAsset nodes (each containing a PropAsset), CurveAsset, PatternAsset, ScannerAsset
- **Output**: Props placed in clustered pattern
- **Use Case**: Trees in groves, rock formations, grouped decorations

### ColumnPropAsset
- **Node Name**: ColumnPropAsset
- **Purpose**: Places props as vertical columns (stacked blocks)
- **Parameters**:
  - ColumnBlocks (ColumnBlock[]): Array of column block definitions
    - Each has: Y (int): Relative height, Material (MaterialAsset): Block type
  - BlockMask (BlockMaskAsset): Block mask for placement
  - Directionality (DirectionalityAsset): Direction settings (Random, Static, Orthogonal, Pattern)
  - Scanner (ScannerAsset): Scanner for finding positions
- **Child Assets**: Multiple ColumnBlock nodes (each with MaterialAsset), BlockMaskAsset, DirectionalityAsset, ScannerAsset
- **Output**: Vertical column props
- **Use Case**: Logs, pillars, stalactites, icicles

### DensityPropAsset
- **Node Name**: DensityPropAsset
- **Purpose**: Places props based on density values (probability-based)
- **Parameters**:
  - Range (Vector3i): X/Y/Z range for placement
  - PlacementMask (BlockMaskAsset): Where props can be placed
  - Pattern (PatternAsset): Pattern for placement
  - Scanner (ScannerAsset): Scanner for finding positions
  - Density (DensityAsset): Density determining placement likelihood
  - Material (MaterialProviderAsset): Material to place
- **Child Assets**: BlockMaskAsset, PatternAsset, ScannerAsset, DensityAsset, MaterialProviderAsset
- **Output**: Props placed based on density probability

### ImportedPropAsset
- **Node Name**: ImportedPropAsset
- **Purpose**: References imported prop definition
- **Parameters**: Asset reference
- **Child Assets**: None

### NoPropAsset
- **Node Name**: NoPropAsset
- **Purpose**: Places no props (placeholder/do-nothing)
- **Parameters**: None
- **Child Assets**: None
- **Output**: No props placed

### PondFillerPropAsset
- **Node Name**: PondFillerPropAsset
- **Purpose**: Fills pond areas with liquid blocks
- **Parameters**:
  - Liquid (MaterialProviderAsset): Liquid material to place
  - Pattern (PatternAsset): Where ponds are detected
- **Child Assets**: MaterialProviderAsset, PatternAsset
- **Output**: Liquids filling pond depressions

### PrefabPropAsset
- **Node Name**: PrefabPropAsset
- **Purpose**: Places prefab structures as props
- **Parameters**:
  - Prefab (PrefabReference): Prefab to place
  - Directionality (DirectionalityAsset): Direction settings
  - Placement (Vector3d): Position offset
- **Child Assets**: Prefab reference, DirectionalityAsset
- **Output**: Prefab structure placed

### QueuePropAsset
- **Node Name**: QueuePropAsset
- **Purpose**: Queues multiple prop placements in sequence
- **Parameters**:
  - Props (PropAsset[]): Array of prop providers
- **Child Assets**: Multiple PropAsset nodes
- **Output**: Props placed in queue order

### UnionPropAsset
- **Node Name**: UnionPropAsset
- **Purpose**: Combines multiple prop providers
- **Parameters**:
  - Props (PropAsset[]): Array of prop providers
- **Child Assets**: Multiple PropAsset nodes
- **Output**: Combined placements from all props

---

## TINT PROVIDER NODES

**Purpose**: Tint Provider nodes determine color tints applied to blocks/materials.

### ConstantTintProviderAsset
- **Node Name**: ConstantTintProviderAsset
- **Purpose**: Always returns constant color tint
- **Parameters**:
  - Color (Color): RGBA color value (default #FF0000 red)
    - Format: #RRGGBBAA hex or similar
- **Child Assets**: None
- **Output**: Constant tint color

### DensityDelimitedTintProviderAsset
- **Node Name**: DensityDelimitedTintProviderAsset
- **Purpose**: Provides tints based on density value ranges
- **Parameters**:
  - Ranges (TintRange[]): Array of density-to-tint mappings
    - Each has: Min (double), Max (double), TintProviderAsset
- **Child Assets**: DensityAsset, multiple TintProviderAsset nodes
- **Output**: Tint based on which density range position falls in
- **Use Case**: Height-based vegetation colors, biome gradients

### NoTintProviderAsset
- **Node Name**: NoTintProviderAsset
- **Purpose**: Provides no tint (uses default/un-tinted colors)
- **Parameters**: None
- **Child Assets**: None
- **Output**: No tint (default colors)

---

## VECTOR PROVIDER NODES

**Purpose**: Vector Provider nodes provide 3D direction/offset vectors used in various operations.

### CacheVectorProviderAsset
- **Node Name**: CacheVectorProviderAsset
- **Purpose**: Caches vector provider results for performance
- **Parameters**: Cache parameters
- **Child Assets**: VectorProviderAsset
- **Output**: Cached vector values

### ConstantVectorProviderAsset
- **Node Name**: ConstantVectorProviderAsset
- **Purpose**: Always returns constant vector value
- **Parameters**:
  - Value (Vector3d): X/Y/Z vector components
- **Child Assets**: None
- **Output**: Constant vector

### DensityGradientVectorProviderAsset
- **Node Name**: DensityGradientVectorProviderAsset
- **Purpose**: Returns gradient vector of density at position
- **Parameters**:
  - Density (DensityAsset): Density to sample
- **Child Assets**: DensityAsset
- **Output**: Gradient vector (∂density/∂x, ∂density/∂y, ∂density/∂z)
- **Use Case**: Flow directions, slope vectors

### ExportedVectorProviderAsset
- **Node Name**: ExportedVectorProviderAsset
- **Purpose**: References exported vector provider
- **Parameters**: Reference identifier
- **Child Assets**: None

### ImportedVectorProviderAsset
- **Node Name**: ImportedVectorProviderAsset
- **Purpose**: References imported vector provider
- **Parameters**: Asset reference
- **Child Assets**: None

---

## ENVIRONMENT PROVIDER NODES

**Purpose**: Environment Provider nodes determine environment settings (weather, fog, etc.) for biomes.

### ConstantEnvironmentProviderAsset
- **Node Name**: ConstantEnvironmentProviderAsset
- **Purpose**: Always returns constant environment type
- **Parameters**:
  - Environment (String): Environment asset name (default "Unknown")
- **Child Assets**: None
- **Output**: Constant environment settings

### DensityDelimitedEnvironmentProviderAsset
- **Node Name**: DensityDelimitedEnvironmentProviderAsset
- **Purpose**: Provides environment based on density values
- **Parameters**:
  - Ranges (EnvironmentRange[]): Array of density-to-environment mappings
    - Each has: Min (double), Max (double), EnvironmentProviderAsset
- **Child Assets**: DensityAsset, multiple EnvironmentProviderAsset nodes
- **Output**: Environment based on which density range position falls in
- **Use Case**: Elevation-based weather, altitude-based fog

---

## TERRAIN NODES

**Purpose**: Terrain nodes define how the terrain height/shapes are determined for a biome.

### DensityTerrainAsset
- **Node Name**: DensityTerrainAsset
- **Purpose**: Terrain defined by density function
- **Parameters**:
  - Density (DensityAsset): Density defining terrain shape
- **Child Assets**: Single DensityAsset node
- **Output**: Terrain height determined by density
- **Use Case**: Most common terrain type, custom terrain shapes

### TerrainAsset
- **Node Name**: TerrainAsset
- **Purpose**: Base class for terrain definitions
- **Parameters**: Varies by subclass
- **Child Assets**: Varies by subclass

---

## SCANNER NODES

**Purpose**: Scanner nodes determine how to search for positions where props/decorations can be placed.

### AreaScannerAsset
- **Node Name**: AreaScannerAsset
- **Purpose**: Scans for positions in an area shape (circle/square)
- **Parameters**:
  - ResultCap (int): Maximum results to return (>= 0)
  - ScanShape (ScanShape): Shape to scan
    - Values: CIRCLE, SQUARE
  - ScanRange (int): Scan radius/size (>= -1, -1 = unlimited)
  - ChildScanner (ScannerAsset): Scanner to use at each point
- **Child Assets**: Single ScannerAsset node
- **Output**: Positions within area
- **Use Case**: Finding all suitable spots within radius

### ColumnLinearScannerAsset
- **Node Name**: ColumnLinearScannerAsset
- **Purpose**: Scans columns in linear pattern from surface
- **Parameters**: Column scanning parameters (direction, depth)
- **Child Assets**: ScannerAsset
- **Output**: Linear column positions

### ColumnRandomScannerAsset
- **Node Name**: ColumnRandomScannerAsset
- **Purpose**: Scans columns randomly from possible positions
- **Parameters**: Column scanning parameters (probability, depth)
- **Child Assets**: ScannerAsset
- **Output**: Randomly selected column positions

### ImportedScannerAsset
- **Node Name**: ImportedScannerAsset
- **Purpose**: References imported scanner
- **Parameters**: Asset reference
- **Child Assets**: None

### OriginScannerAsset
- **Node Name**: OriginScannerAsset
- **Purpose**: Returns origin position only (single point)
- **Parameters**: None
- **Child Assets**: None
- **Output**: Single position at origin (0,0,0)

---

## PROP ASSIGNMENT NODES

**Purpose**: Prop Assignment nodes determine how props are distributed to placement positions.

### AssignmentsAsset
- **Node Name**: AssignmentsAsset
- **Purpose**: Base class for prop assignments
- **Parameters**: Varies by subclass
- **Child Assets**: Varies by subclass

### ConstantAssignmentsAsset
- **Node Name**: ConstantAssignmentsAsset
- **Purpose**: Always assigns same prop to all positions
- **Parameters**:
  - Prop (PropAsset): Prop to assign
- **Child Assets**: Single PropAsset node
- **Output**: Same prop for all positions

### FieldFunctionAssignmentsAsset
- **Node Name**: FieldFunctionAssignmentsAsset
- **Purpose**: Assigns props based on field function (density)
- **Parameters**: Field function parameters with prop mappings
- **Child Assets**: DensityAsset
- **Output**: Prop based on density value

### ImportedAssignmentsAsset
- **Node Name**: ImportedAssignmentsAsset
- **Purpose**: References imported assignments
- **Parameters**: Asset reference
- **Child Assets**: None

### SandwichAssignmentsAsset
- **Node Name**: SandwichAssignmentsAsset
- **Purpose**: Assigns props in sandwich layers (top/fill/bottom)
- **Parameters**:
  - Top (PropAsset): Material/prop for top layer
  - Fill (PropAsset): Material/prop for middle filling
  - Bottom (PropAsset): Material/prop for bottom layer
- **Child Assets**: Multiple PropAsset nodes
- **Output**: Layered prop placements

### WeightedAssignmentsAsset
- **Node Name**: WeightedAssignmentsAsset
- **Purpose**: Randomly assigns props based on weights
- **Parameters**:
  - WeightedProps (WeightedProp[]): Array of weighted props
    - Each has: Weight (double), PropAsset
- **Child Assets**: Multiple PropAsset nodes
- **Output**: Randomly selected prop based on weights

---

## POINT GENERATOR NODES

**Purpose**: Point Generator nodes create point distributions used by other systems.

### MeshPointGeneratorAsset
- **Node Name**: MeshPointGeneratorAsset
- **Purpose**: Generates points in mesh pattern
- **Parameters**: Mesh parameters (spacing, dimensions)
- **Child Assets**: None
- **Output**: Array of mesh points

### NoPointGeneratorAsset
- **Node Name**: NoPointGeneratorAsset
- **Purpose**: Generates no points (empty result)
- **Parameters**: None
- **Child Assets**: None
- **Output**: Empty point array

---

## NOISE GENERATOR NODES

**Purpose**: Noise Generator nodes are base classes for noise creation.

### CellNoiseAsset
- **Node Name**: CellNoiseAsset
- **Purpose**: Cellular/Worley noise generator
- **Parameters**: Cell noise parameters
- **Child Assets**: None
- **Output**: Cellular noise values

### NoiseAsset
- **Node Name**: NoiseAsset
- **Purpose**: Base class for noise generators
- **Parameters**: Varies by subclass
- **Child Assets**: Varies by subclass

### SimplexNoiseAsset
- **Node Name**: SimplexNoiseAsset
- **Purpose**: Simplex noise generator
- **Parameters**: Simplex noise parameters
- **Child Assets**: None
- **Output**: Simplex noise values

---

## OTHER ASSET NODES

**Purpose**: Other utility asset nodes for block management.

### BlockMaskAsset
- **Node Name**: BlockMaskAsset
- **Purpose**: Defines which blocks can be used for placement restrictions
- **Parameters**:
  - Entries (BlockMaskEntry[]): Array of block types
- **Child Assets**: Multiple BlockMaskEntryAsset nodes
- **Output**: Block mask filter
- **Use Case**: Restricting props to specific block types

### MaterialSetAsset
- **Node Name**: MaterialSetAsset
- **Purpose**: Collection of material definitions
- **Parameters**:
  - Materials (MaterialAsset[]): Array of material entries
- **Child Assets**: Multiple MaterialAsset nodes
- **Output**: Set of materials

---

## SUB-NODE CATEGORIES

**Purpose**: Helper sub-nodes used by primary node categories.

### Density Return Type Nodes (for CellNoise)
Used to determine what value cellular noise returns:
- **CellValueReturnTypeAsset**: Returns cell center value
- **CurveReturnTypeAsset**: Maps cell value through curve
- **DensityReturnTypeAsset**: Samples another density at cell position
- **DistanceReturnTypeAsset**: Returns distance to cell center
- **Distance2AddReturnTypeAsset**: Returns distance squared plus value
- **Distance2DivReturnTypeAsset**: Returns distance squared divided by value
- **Distance2MulReturnTypeAsset**: Returns distance squared multiplied by value
- **Distance2SubReturnTypeAsset**: Returns distance squared minus value
- **Distance2ReturnTypeAsset**: Returns distance squared
- **ImportedReturnTypeAsset**: References imported return type

### Distance Function Nodes (for CellNoise)
Distance calculation methods:
- **EuclideanDistanceFunctionAsset**: Standard Euclidean distance (straight line)
- **ManhattanDistanceFunctionAsset**: Manhattan/taxicab distance (grid-aligned)

### SpaceAndDepth Layer Nodes (for Material Providers)
Layer definitions for space-based material providers:
- **ConstantThicknessLayerAsset**: Layer with constant thickness
- **NoiseThicknessAsset**: Layer thickness based on noise
- **RangeThicknessAsset**: Layer thickness in specified range
- **WeightedThicknessLayerAsset**: Layer with weighted material selection

### SpaceAndDepth Condition Nodes (for Material Providers)
Conditional logic for space-based material providers:
- **AlwaysTrueConditionAsset**: Always true
- **AndConditionAsset**: Logical AND of conditions
- **EqualsConditionAsset**: Equality check
- **GreaterThanConditionAsset**: Greater than comparison
- **NotConditionAsset**: Logical NOT
- **OrConditionAsset**: Logical OR of conditions
- **SmallerThanConditionAsset**: Less than comparison

### Directionality Nodes (for Props)
Direction settings for prop placement:
- **DirectionalityAsset**: Base class
- **StaticDirectionalityAsset**: Fixed direction
- **RandomDirectionalityAsset**: Random direction
- **OrthogonalDirectionAsset**: Cardinal directions (N/S/E/W)
- **PatternDirectionalityAsset**: Pattern-based directions
- **RotatedPositionsDirectionality**: Rotated positions

---

## NODE CONNECTION RULES

### Input/Output Types
- **Density**: Input → Density output (double)
- **Curve**: Input → Curve output (double)
- **Pattern**: Position → Pattern output (boolean)
- **MaterialProvider**: Position/Context → Material output
- **PositionProvider**: Context → Position(s) output
- **Prop**: Context → Prop placement
- **TintProvider**: Position → Color output
- **VectorProvider**: Position → Vector output
- **EnvironmentProvider**: Position → Environment output
- **Scanner**: Position → Position(s) output
- **Assignments**: Position → Prop(s) output

### Common Input Parameters
All nodes can have:
- **Skip** (boolean): If true, node returns default/empty value
- **ExportAs** (string): Optional name for reusing this node elsewhere

### Validation Rules
Common validators applied to parameters:
- **greaterThan(x)**: Must be > x
- **greaterThanOrEqual(x)**: Must be >= x
- **non-zero**: Cannot be zero vector
- **non-negative**: Cannot be negative

---

## USAGE EXAMPLES

### Creating a Simple Terrain Height Map
```
1. SimplexNoise2dDensityAsset (base noise)
   ↓
2. ScaleDensityAsset (stretch to world size)
   ↓
3. TerrainDensityAsset (use as terrain)
```

### Creating a Biome with Materials
```
1. SimplexNoise2dDensityAsset (height variation)
   ↓
2. DensityTerrainAsset (terrain shape)
   ↓
3. WeightedMaterialProviderAsset (select blocks randomly)
   ↓
4. BiomeAsset (combine terrain + materials)
```

### Placing Props on Surfaces
```
1. OriginScannerAsset (start at origin)
   ↓
2. SurfacePatternAsset (find surface)
   ↓
3. ColumnPropAsset (place trees as columns)
   ↓
4. ClusterPropAsset (group trees)
```

---

## QUICK REFERENCE TABLE

| Category | Purpose | Typical Input | Output |
|----------|---------|---------------|--------|
| Density | Height/probability fields | Position | Double |
| Curve | Value transformation | Double | Double |
| Pattern | Block matching | Position | Boolean |
| MaterialProvider | Block selection | Position | Material |
| PositionProvider | Position generation | Context | Position(s) |
| Prop | Decoration placement | Context | Placements |
| TintProvider | Color tinting | Position | Color |
| VectorProvider | Direction/offset | Position | Vector |
| EnvironmentProvider | Environment settings | Position | Environment |
| Terrain | Terrain definition | - | Height |
| Scanner | Position search | Context | Position(s) |
| Assignments | Prop distribution | Position | Prop(s) |

---

## EXPORT INFORMATION

This documentation is structured in Markdown format for easy export and reference. It can be:

1. **Copied directly** into documentation systems
2. **Converted to other formats** (HTML, PDF) using Markdown converters
3. **Used as LLM reference** for biome/world generation tasks
4. **Integrated into IDEs** with Markdown support

**Total Nodes Documented**: 100+ across 22 categories
**Coverage**: Complete - all node types in visual editor system

---

*Document Version*: 1.0
*Generated from*: HytaleServer decompiled codebase
*Date*: 2025-01-19
