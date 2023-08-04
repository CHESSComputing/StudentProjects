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
- Cycle (can be found in the spec file's path, or stored in the `CURRENTCYCLE` global in SPEC, set whenever spec starts (whenever userlist.mac is run))
- BTR (can be found in the spec file's path, or stored in the `BTRID` global in SPEC, set by the `newuser` macro)
- Beamline (`"3A"` for this case, or can be found in the spec file's path in the general case, or the `BEAMLINE` global at id1a3 and id3a)
- DataLocation[Raw|Meta|Reduced|Scratch|BeamtimeNotes] (from globals in SPEC: `[RAW|META|REDUCED|SCRATCH]USERLN` and `BEAMTIMENOTE`, set when the `newuser` macro is run).
- BeamEnergy
  - Run the `getE` macro, _then_ the `LAMBDA` global (in Angstromms) can be used
  - NB `LAMBDA` is not 100% guaranteed to be up-to-date until after `getE` is run
- Technique
  - `"tomography"` for this case
  - In general: _may_ be set on a scan-by-scan basis.
  - 1a3 & 3a have mode-switching macros, but the mode selected is not (currently) stored in any SPEC globals.
- SampleName (`SAMPLENAME`, set when `newsample` is run)
- PI (`USERNAME`)

Fields that will require manual input:
- Set at level of `newuser`:
  - Experimenters (free text)
  - StaffScientist (limited menu of options)
  - CESRConditions (limited menu of options)
- Set at the level of `newsample`:
  - Reference[Calibrant|Energy][SampleName|ScanNumber]
    - All 4 fields above are optional -- we can leave them out for now.
  - Sample[CommonName|UnitCell|SpaceGroup|Geometry|MatPedHeatTreatment|MatPedProcessingRoute], MaterialSafetyHazardousSamples
    - Requires manual input, not optional.
    - However: could make them optional? Ask KS & AD if this is OK for now.

Unsure:
- Alignment
  - For tomo: always false
- EnergyScan
  - For tomo: always false
- UndulatorScan
  - For tomo: always false
- [Beam|Pre|Gurard]Slit[Horizontal|Vertical][Size|Position] (12 fields total)
  - NB: No "Pre" slit at 3A! Schema should be updated -- only 8 fields total
  - Get these from transforming the individual blade motor positions: at id3a, account for slit box & individual blades (the usual slit macro motors are not used here).
- Monochromator
  - Can be determined from within SPEC -- ask KS & AD what to use
- Focusing
  - Optional & N/A for tomo -- use "None"
- Atten[Material|Thickness]
  - Expression of 2 motors: `A[att]*1+A[attf]*0.25` where `att` & `attf` can be hard-coded.
- EnergyFoil
  - Schema needs updating for 3A -- list of options is not correct.
  - Optional, and not needed for tomo. Leave it out.
- Detectors
  - For tomo at 3a: either Retiga or Manta.
  - In SPEC, the global `SYNC_NF_DETECTOR` holds the detector name (all lowercase, though)
- ExperimentType
  - For tomo: always "Imaging"
- InSitu
  - For tomo: always false
- MechanicalTest
  - Intent needs clarification: is this tomo scan _part_ of a mech. test?
  - If _part_ of a mech. test -> true, set at the level of `newsample`
- MechanicalTestType
  - For tomo: leave as empty string
  - set at the level of `newsample`
- MechanicalLoadFrame
  - Even if not doing a mech test, sample could still be mounted in a load frame.
  - set at the level of `newuser`
- MechanicalGrips
- SupplementaryTechnique
- Furnace
- Processing
- Calibration
