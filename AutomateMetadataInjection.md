# Automate Metadata Collection for Tomography at ID3A
Construct & submit JSON records to the CHESS Metadata Service automatically when tomography scans are run at ID3A.
Needed by the NSDF dashboard to effectively present which datasets are available for visualization.

This will likely be achieved by adding pieces to existing spec macros,
but new macros that propmpt the SPEC user for manual input of some metadata fields may need to be introduced
(these prompt-for-manual-input macros would be used less frequently than the existing macros like `newsample` and `newfile`,
and simply store the users' inputs in some global variable(s) in SPEC.). 

[ID3A metadata schema](https://github.com/vkuznet/ChessDataManagement/blob/master/web/schemas/ID3A.json)

Fields that can be determined automatically from within spec when a tomography scan is run:
- Facility (`"CHESS"`, always)
- Cycle (can be found in the spec file's path)
- BTR (can be found in the spec file's path)
- Beamline (`"3A"` for this case, or can be found in the spec file's path in the general case)
- DataLocation[Raw|Meta|Reduced|Scratch|BeamtimeNotes]
- BeamEnergy
- Technique (`"tomography"` for this case, not sure about the general case)
- SampleName

Fields that will require manual input:
- PI
- Experimenters
- StaffScientist
- CESRConditions
- Reference[Calibrant|Energy][SampleName|ScanNumber]
- Sample[CommonName|UnitCell|SpaceGroup|Geometry|MatPedHeatTreatment|MatPedProcessingRoute]
- MaterialSafetyHazardousSamples

Unsure:
- Alignment
- EnergyScan
- UndulatorScan
- [Beam|Pre|Gurard]Slit[Horizontal|Vertical][Size|Position] (12 fields total)
- Monochromator
- Focusing
- Atten[Material|Thickness]
- EnergyFoil
- Detectors
- ExperimentType
- InSitu
- MechanicalTest
- MechanicalTestType
- MechanicalLoadFrame
- MechanicalGrips
- SupplementaryTechnique
- Furnace
- Processing
- Calibration
