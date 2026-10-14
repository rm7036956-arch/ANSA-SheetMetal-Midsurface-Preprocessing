# ANSA-SheetMetal-Preprocessing ( Project 1 ) 
Item	Details
Component	Front floor panel (Hyundai Sonata), stamped sheet metal
Software	ANSA (BETA CAE Systems)
Input format	STEP (.stp)
Mid-surface method	Constant-thickness (offset-based)
Shell thickness	1 mm (constant)
Target element length	5 mm
Total shell elements	45,436
Quad elements	43,824 (96.45 %)
Tria elements	1,612 (3.55 %, limit was 10 %)
Application	BIW structural, crash and NVH simulation
1. Objective
Reproduce an industry-style pre-processing workflow on a real automotive part, starting from raw CAD and ending with a quality-checked, analysis-ready shell mesh.
The task required:

Extracting the mid-surface from the CAD model
Cleaning up the geometry (topology repair, defeaturing, closing gaps)
Meshing at a 5 mm target element length
Meeting a strict set of mesh quality criteria
Assigning shell thickness for the sheet-metal gauge

2. About the Component
The part is a stamped sheet-metal front floor panel.
It forms the passenger footwell and connects to the tunnel, side sills and cross-members of the Body-in-White.
It has embossed stiffening ribs, a raised central tunnel, bead patterns, welding flanges, and several mounting and access holes. 
All of these features had to be kept accurate in the mid-surface and the mesh.

3.Workflow
STEP import  →  Topology clean-up  →  Mid-surface extraction  →  Mid-surface defeaturing  →  2D shell meshing  →  Quality check & thickness

Stage 1: Geometry Import and Topology Clean-up
Unchecked faces 
Needle faces
Crack between CONS
Triple CONS
Single CONS

2: Mid-Surface Extraction
Method: constant-thickness (offset-based) extraction, 1 mm thickness.
Why this method:
The panel is a stamped part with a uniform 1 mm gauge, so one offset surface is enough.
Variable-thickness mid-surfacing is meant for castings or plastic parts with several wall thicknesses. Here it would only add computation and surface-deviation risk with no accuracy benefit.
A constant offset keeps the shell mesh centred, which matters for correct bending stiffness in crash and NVH analysis.

Stage 3: Mid-Surface Clean-up and Defeaturing
The offset is computed face by face, so the raw mid-surface has overlapping patches at fillets, ribs and flanges. These ANSA tools were used, iteratively, to rebuild one clean surface:

Tool	                    Purpose
Split                     Break multi-edged surfaces into simple 3/4-sided patches
Improve / Reconstruct	    Rebuild distorted or self-intersecting patches at ribs and fillets
Reshape                 	Regenerate cleaner surfaces from the trial mesh
Join	                    Merge unnecessary splits on flat regions to improve mesh flow
Length From Perimeter	    Derive consistent local element size from patch boundaries

Stage 4: 2D Shell Meshing
Meshing was done on the cleaned mid-surface, quad-dominant, at 5 mm. Every region had to pass the quality criteria below.

Stage 5: Quality Verification and Thickness Assignment
The ANSA Quality Monitor was used live during meshing, not only at the end. 1 mm thickness was assigned to the shell property.

4. Mesh Quality Criteria
S.N	Criterion	                Target	           Result
1	Aspect ratio	                 5	             Pass
2	Skewness	                     45              Pass
3	Warping	                       15	             Pass
4	Taper                       	0.5	             Pass
5	Min. length                 	2 mm	           Pass
6	Max. length	                  8 mm	           Pass
7	Min angle (quad)	             45°	           Pass
8	Max angle (quad)	             135°	           Pass
9	Min angle (tria)	             20°             Pass
10	Max angle (tria)	          120°             Pass
11	Tria %	                   ≤ 10 %	          3.55 %


Mesh-Flow Rules Followed
Besides the numeric limits, the mesh follows BIW best practice:
No trias on free, shared or non-manifold edges
No opposite trias and no back-to-back trias
No trias on feature boundaries (embosses, fillets, holes, welding cylinders, mounting clips)
Even number of nodes around every hole, for symmetric washer rings
No rotational quads and no repeating "pattern trias"



Skills Demonstrated
BIW sheet-metal CAE pre-processing from CAD import to quality-driven meshing
Choosing the right mid-surface method (constant vs. variable thickness) for the manufacturing type
ANSA topology and surface tools: Defeature, Reconstruct, Split, Improve, Reshape, Join, Length From Perimeter
Applying and verifying shell mesh quality metrics
Mesh-flow control around holes, ribs and features

## Conclusion

A quality-compliant, analysis-ready shell mesh of the Sonata front floor panel was generated from raw STEP geometry. All topology defects were fixed before meshing, so they did not carry into the mesh. A constant-thickness mid-surface was chosen because the part is stamped sheet metal with a uniform 1 mm gauge.

The final model has **45,436 shell elements (96.45% quads, 3.55% trias)**. It meets every specified quality criterion, and the tria percentage is well under the 10% limit. The mesh follows BIW mesh-flow practice: no trias on feature edges, even node counts around holes, and clean quad flow through the tunnel and rib bank.

The workflow applies directly to Body-in-White structural, crash and NVH simulation.

## What I Learnt

**Technical**
- Clean geometry comes first.** Needle faces, cracks and free edges must be fixed before meshing. Fixing them later takes much more effort and still leaves a poor mesh.
- Choosing the mid-surface method.** Constant-thickness offset suits uniform-gauge stampings. Variable-thickness methods suit castings and plastic parts with several wall thicknesses.
- Defeaturing for mesh flow.** Joining unnecessary patch splits on flat regions removes the boundaries that force tria transitions, which helped keep tria % low.
- Rebuilding the mid-surface.** Split, Improve, Reshape and Reconstruct turn overlapping offset patches into one clean surface.
- Meshing around features.** Even node counts on holes and quad-only edges around embosses, fillets and welding cylinders keep the mesh symmetric and reliable for later connection modeling.
- Live quality checking.** Using the Quality Monitor during meshing is faster than fixing failed elements at the end.

**Professional**
- Working to a written acceptance criteria table, as in an industry brief
- Documenting each decision with its reason, not just the steps
- Treating a mesh as a deliverable that someone else will use for analysis

**What I would do next**
- Automate the quality check with ANSA Python scripting
- Add connections (spot welds, bolts) and assemble the part with neighbouring BIW components
- Run a simple modal or static analysis to check the model solves cleanly




-------------------------------------------------------------------------------------------------------------------------------------------------------------------
# ANSA-Plastic-Preprocessing ( Project 2 ) 




