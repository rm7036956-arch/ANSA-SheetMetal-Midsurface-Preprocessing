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

Mid-Mesh Generation of a Plastic Car IP Console (ANSA)

A project that converts the CAD of a plastic car instrument panel (IP) console / dashboard into a clean, variable-thickness mid-mesh (shell mesh) ready for structural, NVH or crash analysis.

1. Project Summary
Item	Details
Component	Car IP console / dashboard (instrument cluster area, music-system area, full dashboard)
Material type	Plastic (injection moulded), variable wall thickness
Input file	Dash Board Sub Parts-Final.IGS (IGES)
Software	ANSA (Checks Manager, Mesh, Align Manager, Casting)
Method	Casting method for mid-mesh generation (not mid-surface)

Why mid-mesh? Plastic parts have changing wall thickness around ribs, bosses and clips. The casting method finds the mid-plane directly from the solid and generates the shell mesh and its thickness together, which is faster and more accurate than building a constant-thickness mid-surface.

2. Files in This Package
File	Description
IP_Console_MidMesh_Project_Report.docx	Full project report (editable)
IP_Console_MidMesh_Project_Report.pdf	Same report as PDF (14 pages, ready to share)
DogHouse_Tapered_Before_After.png	Before/after: tapered dog house
DogHouse_Cylindrical_Before_After.png	Before/after: angled cylindrical dog house
DogHouse_Triple_Before_After.png	Before/after: group of three dog houses
DogHouse_4_Tapered_v2_Before_After.png	Before/after: tapered dog house (second example)
DogHouse_5_Twin_Cylinders_Before_After.png	Before/after: twin cylindrical bosses
DogHouse_6_Angled_Flat_Before_After.png	Before/after: angled dog house (curves vs mesh)
README.md	This file
3. Workflow
Import the IGES geometry into ANSA.
Geometry checks with the Checks Manager (Geometry). 371 errors were found.
Geometry cleanup (see Section 4).
Mid-mesh generation with the Casting method (see Section 5).
Dog house capture: features the automatic method missed were rebuilt manually (see Section 6).
Alignment: Align Manager for perimeters, areas and imprints; white lines removed.
Mesh repair and quality: FEM Topo, Smooth, Reshape, Reconstruct, Sweep, Split.
Quality check against the project criteria (see Section 7).
4. Geometry Errors and Fixes
Code	Error	Meaning
E5102	Problematic Surface	Surface with bad mathematical shape
E4068	Unchecked Faces	Faces the checker could not evaluate (26)
E4071	Needle Face	Very thin, sliver-like face
E4072	Overlapping Faces	Two faces in the same place
E4073	Crack Between Cons	Tiny gap between neighbouring faces
E4070	Triple Cons	Three faces sharing one edge
E4074	Single Cons	Free edge belonging to only one face

Fixes used: ANSA auto-fix, Cons to Curve, curve editing (Create, Connect, Modify, Project, Delete), Fill to rebuild faces, then re-running the checks.

5. Casting Parameters
Parameter	Value
Minimum thickness	0.5 mm
Target length	4 mm
Minimum length	2 mm
Maximum length	6 mm
Thickness result (T result)	Taken and treated
6. Dog House Capture

Dog houses are solid, hollow-from-inside features with no wall thickness of their own, so they are captured in the mid-mesh as a cross pattern.

Workflow for each dog house:

Create curves along its edges using Cons to Curve (plus Connect / Create / Modify / Project / Delete).
Build the missing faces with Fill.
Align perimeters and areas to the wall mesh using Align Manager.
Fill remaining gaps with FEM Topo.
Check quality, then Freeze the finished area.

Freeze / Unfreeze: aligning one perimeter can disturb a neighbouring area. Freezing a finished area protects it from later changes; Unfreeze is used when it needs editing again.

7. Results
Mesh statistics (ANSA Quality Monitor)
Item	Count	Share
Total shells	50,641	100 %
Quads	48,167	95.11 %
Trias	2,474	4.89 %
Quality criteria
#	Criterion	Required	Status
1	Target length	4 mm	Pass
2	Minimum length	2 mm	Pass
3	Maximum length	6 mm	Pass
4	Aspect ratio	≤ 3	Pass
5	Warpage	≤ 15°	Pass
6	Skewness	≤ 45°	Pass
7	Jacobian	≥ 0.7	Pass
8	Minimum quad angle	≥ 45°	Pass
9	Maximum quad angle	≤ 135°	Pass
10	Minimum tria angle	≥ 30°	Pass
11	Maximum tria angle	≤ 120°	Pass
12	Tria percentage	≤ 15 %	Pass (4.89 %)

A few failed elements may remain near triple cons or in very small regions bounded by two triple cons; these are kept as they are when they cannot be solved.

8. Tools and Commands Used
Tool	Purpose
Checks Manager (Geometry)	Find geometry errors
Cons to Curve	Convert edges to curves, capture dog houses
Curves: Create / Connect / Modify / Project / Delete	Build and edit boundaries
Faces / Fill	Create faces; convert cons into mesh
Casting	Generate variable-thickness mid-mesh
Extrusion	Mid-surface generation in selected areas
Align Manager	Align perimeters, areas, imprints; remove white lines
Smooth / Reshape / Reconstruct	Improve element quality
FEM Topo	Fill holes and gaps in the mesh
Sweep, Split	Create and split elements
Freeze / Unfreeze	Protect finished areas
9. Editing the Report
Open the .docx in Microsoft Word to edit text, tables and images.
Fill in the Prepared by name on the cover page.
To update quality values with exact measured numbers, edit the "Result" column in Section 9.2.
Export to PDF from Word after editing.
10. Skills Demonstrated
Geometry cleanup and topology repair in ANSA
Variable-thickness mid-mesh generation for plastic parts
Manual capture of complex features (dog houses, clips, bosses)
Mesh quality control to OEM-style criteria







