# ANSA-SheetMetal-Plastic-Midsurface-Preprocessing
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
