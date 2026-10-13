.. _CHANGELOG:

Change Log
##########

Latest blog posts and news are available at the `OSVVM Blog <https://osvvm.org/blog>`__; the changes of each release
are in the ``CHANGELOG.md`` files of OSVVM's repositories, linked from OsvvmLibraries'
`CHANGELOG.md <https://github.com/OSVVM/OsvvmLibraries/blob/main/CHANGELOG.md>`__.

Since 2020.07, a release of OSVVM is a release of `OsvvmLibraries <https://github.com/OSVVM/OsvvmLibraries>`__, which
collects all OSVVM repositories as Git submodules. Earlier releases are releases of the OSVVM utility library
(`OSVVM/OSVVM <https://github.com/OSVVM/OSVVM>`__).


.. _CHANGELOG/2026:

2026
****

.. _CHANGELOG/2026.09:

2026.09
=======

:Released: 2026-10-04
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2026.09>`__

Gilda Radner taught us, "...it is always something ...". OSVVM release 2026.09 addresses some of the small "always something" things so I can move on and worry about the bigger things a little later.

-  Linking of HTML transcripts to the HTML log file - for regressions when not using in mirroring

   -  Fixed constant OsvvmSettingsPkg_default In 2026.08 constant should be named %%x and not %%+

-  For Alert/Log with Wrapped lines,

   -  If start with LF, indent, but only wrap if ALERT_LOG_WRAP
   -  If ALERT_LOG_WRAP, do not wrap unless line is longer than threshold

-  In HTML files added handling of characters '<' and '>' so they are not seen as an HTML tag

   -  Line continuation indication changed from %%> to %%:

-  Calling TestName explicitly in the scripts suspends renaming of test cases with generics

   -  Facilitates formulating names your way

-  When simulate finishes, in tcl, $::osvvm::TestCaseStatus holds simulate completion status (PASSED, FAILED, ...)
-  Fixed file name case sensitivity issue in CoSim \*.pro file (only impacts usage on Linux/Unix)

.. _CHANGELOG/2026.08:

2026.08
=======

:Released: 2026-09-10
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2026.08>`__

OSVVM release 2026.08 adds the following

-  Updated Requirements Settings
-  Multi-line Alert, Log, LogHeader, PrintLine, PrintHeader, ...
-  Updated Settings Methodology for OsvvmSettingsPkg
-  Compile time selection of MemoryPkg storage policy
-  format: a version of to_string that handles inconsistencies
-  SetExpectedAlertCount, IncrementExpectedAlertCount
-  Status: STOPLIMIT
-  Status Tracking with ExpectedStatus and KnownStatus
-  HTML'ized Transcript Files
-  Known Breaking Changes

`For more details, See my blog post on OSVVM.org <https://osvvm.org/archives/2994>`__

.. _CHANGELOG/2026.05:

2026.05
=======

:Released: 2026-06-07
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2026.05>`__

OSVVM release 2026.05 adds the following

-  Script Exit on build done
-  Script Bug Fix: two RunTest when running interactively
-  Script bug: VhdlLsToml
-  Script bug: Simulate and generics
-  Updates to remove Assert WARNING/ERROR
-  CoveragePkg Refactor
-  MemoryGenericPkg Support for 64 bit integers
-  AlertLogPkg Use Parent ID
-  DynamicVectorGenericPkg
-  VHDL-2019 Interface Support
-  Ethernet Bug: Fixed RefXxxClk generation
-  **Breaking Change:** AXI4 Memory is now AXI word width
-  **Breaking Change:** SetAxi4Options/GetAxi4Options

.. rubric:: Script Exit on build done

If ``$::osvvm::ExitOnBuildDone`` is set and neither ``$::osvvm::SimulateInteractive`` nor ``$::osvvm::Debug`` are set, then exit with a code. The code is 1 if there was an error and 0 otherwise.

.. rubric:: Script Bug Fix: two RunTest when running interactively

If simulate/RunTest is not run from within a build (such as interactively) a second run of RunTest will fail due to not finding the file. Reported as issue `issue 107 <https://github.com/OSVVM/OSVVM/issues/107>`__

Issue started due to 2026.01 where if simulate is run interactively it is wrapped in a build so proper directories, results files, and index entry are created.

Simulate was updated to maintain CurrentWorkingDirectory when it is run interactively.

.. rubric:: Script bug: VhdlLsToml

Fixed issues with VhdlLsToml file creation.

.. rubric:: Script bug: Simulate and generics

If an analyze failed and simulate is called with generics, the generics were remaining set for the next simulate.

.. rubric:: Updates to remove Assert WARNING/ERROR

Updated the following:

-  FifoFillPkg_slv.vhd (from osvvm_common) - changed +i to +1
-  WishboneManager.vhd - handling X in to_integer_null_is0
-  bmp_pack.vhd (from osvvm_videobus) - updated write_byte to handle x characters

.. rubric:: CoveragePkg Refactor

CoveragePkg was refactored to separate the singleton implementation from the PT implementation. The PT implementation (in CoveragePtPkg) interacts with the singleton using an ID (which it acquires on demand). The HTML functional coverage reports now include PT created coverage models.

Going forward, only the singleton implementation (in CoveragePkg) will be updated. The PT implementation has a method, GetCoverageID, to retrieve the internal ID. As a result, new singleton functionality can be accessed - with a little work using GetCoverageID.

This release adds singleton methods WriteAllBins and GetNumIDs. The file ID used by FileOpenWriteBin, named OsvvmCoverageWriteBinFile, is now in the declarative part of CoveragePkg (and hence visible).

The updated PT implementation passes all existing OSVVM test cases without modification, so backward compatibility is expected.

.. rubric:: MemoryGenericPkg Support for 64 bit integers

Updated to support 64 bit implementations (as well as 32 bits) and to automatically select the size based the size your simulator is currently using.

.. rubric:: AlertLogPkg Use Parent ID

Most OSVVM data structures call AlertLogPkg.NewID to create an AlertLogID. A new ReportMode, USE_PARENT_ID was added. This allows data structures to use the parent ID rather than creating an ID. This is appropriate for FIFOs where the only message is a FAILURE (and stop).

.. rubric:: DynamicVectorGenericPkg

A dynamic vector is a resizable vector. Like a vector it supports random access to elements via indices. The indices are numbered 0 to N-1, where N is the number of elements in the vector. As a data structure, it supports insertion and deletion, but it is optimized for insertion and deletion at the end of the vector (index N-1).

Dynamic vectors are implemented in a singleton data structure and can be passed around using their ID. This makes suitable for handling burst transfer information – particularly if the bursts are different sizes. As a singleton, different aspects of the verification environment such as packet generator, verification component (VC) driving the packet, and VC receiving and checking the packet can all reference the packet of data using its ID.

Dynamic vectors have similarities to C/Rust/Ada vectors, SystemVerilog dynamic arrays, Python lists, and Java VectorLists.

This release introduces dynamic vectors. Upcoming releases will integrate them into the OSVVM model independent transaction interfaces.

.. rubric:: VHDL-2019 Interface Support

Currently MIT address bus interface now supported by NVC and Aldec RivieraPRO. Added VHDL-2019 interfaces to Interrupt Handler.

.. rubric:: Ethernet

Bug fix for generation of RefGtxClk. The divide by 2 was missing.

.. rubric:: Breaking Change: AXI4 Memory is now AXI word width

AXI4 Memory is now AXI word width. This better utilizes the memory (meaning uses less workstation memory) and takes advantages of past updates to the MemoryGenericPkg.

If accessing the memory directly by using NewID in another VC, please update the call the NewID as follows:

.. code-block:: VHDL

   vMemID := NewID(
     Name       => MEMORY_NAME,
     AddrWidth  => AXI_ADDR_WIDTH - AXI_BYTE_ADDR_WIDTH,  -- was AXI_ADDR_WIDTH
     DataWidth  => AXI_DATA_WIDTH,  -- was 8
     ParentID   => ParentID,
     Search     => NAME
   ) ;

.. rubric:: Breaking Change: SetAxi4Options/GetAxi4Options

SetAxi4Options/GetAxi4Options for settings other than an AXI4 bus value (such as BRESP) now interact directly with the internal settings of the VC rather than going through the transaction interface.

As a result, if a test case calls SetAxi4Options/GetAxi4Options at time 0 with a setting other than an AXI4 bus value (such as BRESP) the test case will crash. Please add a WaitForClock(TRec, 0) before the first SetAxi4Options/GetAxi4Options call. For more `see the blog post. <https://osvvm.org/archives/2944>`__

.. code-block:: VHDL

   ManagerProc : process
     variable IntOption  : integer ;
   begin
     WaitForClock(ManagerRec, 0) ;
     SetAxi4Options(ManagerRec, WRITE_RESPONSE_READY_DELAY_CYCLES, 2) ;

.. _CHANGELOG/2026.01:

2026.01
=======

:Released: 2026-03-01
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2026.01>`__

.. rubric:: CoSim

-  Adds CoSimPCIe Verification Component - Thanks to Simon Southwell

.. rubric:: MIT

-  Added DoDirectiveTransactions (AddressBus and Stream) to implement common directive transactions
-  Added VHDL-2019 interface capability to AddressBus.
-  Support for interfaces in Stream deferred to later revision due to vendor support issues in many simulators.

.. rubric:: AXI4 Full VC

-  Added VHDL-2019 interfaces
-  Architectures are now separate from the entity in Axi4Manager_a.vhd, Axi4Subordinate_a.vhd, and Axi4Memory_a.vhd
-  If you need to update architectures, only update the 2019 architecture (in src) and enable TCL variable OsvvmDevDeriveArchitectures "true" in OsvvmSettingsLocal.tcl

.. rubric:: ScoreboardGenericPkg

-  Updated FindAndFlush and FindAndDelete s.t. if the item is found, it reports as a PASSED and otherwise as FAILED

.. rubric:: TestCase Reports

-  Opening VHDL Test Case File in vscode, in OsvvmSettingsLocal.tcl set:
   ``variable VhdlFileViewerPrefix "vscode://file/" ;``

   -  Default, view in browser
   -  Assumption, test case file is the one compiled just before simulate

-  For VHDL-2019 simulators, integrate VHDL Assert counts into test pass/fail and Alert reporting (AlertLogPkg)

.. rubric:: Language Support Package

-  Added Constant: TOOL_USES_32_BIT_INTEGERS

.. rubric:: Updated build error handling

-  By default OSVVM catches all errors and ending with a Tcl error is off.
-  This is desirable as when running CI, you want all test cases that can run to run.
-  When running CI, use the JUnit test reporter to generate pass/fail messages
-  Add the following tcl variables to your OsvvmSettingsLocal.tcl to adjust TCL error signaling:

   -  FailOnBuildErrors - Signal a tcl error if an error was signaled during a build. Default is false.
   -  FailOnReportErrors -Signal a tcl error if an error was signaled during reporting. Default is false.
   -  FailOnTestCaseErrors - Signal a tcl error any test case fails during a build. Default is false.
   -  AnalyzeErrorStopCount - if 0, do not signal Analyze errors, otherwise, signal analyze error (and stop) when AnalyzeErrorStopCount errors occur. Default is 0.\*
   -  SimulateErrorStopCount - if 0, do not signal Simulate errors, otherwise, signal simulate error (and stop) when SimulateErrorStopCount errors occur. Default is 0\*

-  If you have errors in any part of the OSVVM process, adjust the following TCL variables

   -  TclDebug - When true and an OSVVM API command fails, print Tcl's $::errorInfo when an error occurs. Default false.
   -  ReportDebug - When true and OSVVM reporting fails, print Tcl's $::errorInfo when an error occurs. Default false.

.. note::

   SetInteractiveMode will change both AnalyzeErrorCount and SimulateErrorCount to 1 and Debug to true (which the same impact as TclDebug).

.. rubric:: Build Reporting

-  If TestSuite has 0, 0, 0 => status is EMPTY
-  If compile before fails, do not start the test case.
-  Simulate error handling

   -  IF compile before fails, do not start. Use error message as message to YAML to display
   -  If simulate fails, use error message to YAML to display.

-  If analyze fails before simulate, test case will FAIL and report Analyze Failed

.. rubric:: Compile Lists as TOML and CSV List

-  CreateDryRunDict - Instead of running analyze and simulate, collect file list and simulate list as dictionaries.
-  CreateAnalyzeListCsv - Run in DryRun and produce a inorder CSV list of files

   -  Closes https://github.com/OSVVM/OSVVM-Scripts/issues/62

-  CreateVhdlLsToml - Run in DryRun and produce VHDL LS Toml files.

   -  Closes https://github.com/OSVVM/OSVVM-Scripts/issues/72

.. rubric:: SPI

-  Updated SPI test suite so that it is passing again

.. rubric:: Scripts

-  Added VHDL-2019 feature controls. These are set automatically in the ``VendorScripts_***.tcl`` file.
-  SetVHDLVersion 2019 automatically done for tools that support it.

   -  Turn off by doing SetVHDLVersion 2008 in your ``OsvvmSettingsLocal.tcl``

.. rubric:: Report Stuff

-  Yaml updates to better collaborate with https://github.com/edaa-org/pyEDAA.OSVVM


.. _CHANGELOG/2025:

2025
****

.. _CHANGELOG/2025.06a:

2025.06a
========

:Released: 2025-09-21
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.06a>`__

.. _CHANGELOG/2025.06:

2025.06
=======

:Released: 2025-07-19
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.06>`__

.. attention::

   The scripts in this release have some issues. Please use :ref:`2025.06a <CHANGELOG/2025.06a>` instead.

.. _CHANGELOG/2025.04:

2025.04
=======

:Released: 2025-05-06
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.04>`__

.. _CHANGELOG/2025.02:

2025.02
=======

:Released: 2025-02-26
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.02>`__


.. _CHANGELOG/2024:

2024
****

.. _CHANGELOG/2024.11:

2024.11
=======

:Released: 2024-12-07
:Source: `Git tag 2024.11 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.11>`__

.. _CHANGELOG/2024.09:

2024.09
=======

:Released: 2024-09-04
:Source: `Git tag 2024.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.09>`__

.. _CHANGELOG/2024.07:

2024.07
=======

:Released: 2024-08-01
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2024.07>`__

The 2024.07 release adds:

-  Breaking changes in Test Status and CreateClock

   -  To maintain backward compabitility, set the script variable OsvvmVersionCompatibility to a value 2024.05 or less in OsvvmSettingsLocal.tcl.
   -  OSVVM provides OsvvmSettingsLocal_example.tcl that can be used to create your OsvvmSettingsLocal.tcl
   -  AlertLogPkg and TbUtilPkg below show individual ways to override these.
   -  :red:`Note however, if you used selected paths to these procedures, it is a breaking change as the paths have changed from "OSVVM.TbUtilPkg.CreateClock" to "OSVVM.CreateClockPkg.CreateClock"`
   -  Note there are also breaking changes related to TbUtilPkg and named association of Polarity input. See TbUtilPkg below for details.

-  AlertLogPkg

   -  Test Status now reports as: PASSED, FAILED, NOCHECKS, TIMEOUT.
   -  By default status NOCHECKS is FAILED. Set script variable FailOnNoChecks to false in OsvvmSettingsLocal.tcl to make it PASSED (old behavior)
   -  PathTail now adds generate label to the name it creates.

-  TbUtilPkg

   -  Added predefined barrier signals: OsvvmTestInit, OsvvmResetDone, OsvvmTestDone, TestDone, OsvvmVcInit
   -  All procedures that have a Clk input now have a ClkActive input
   -  The polarity input on procedures has changed name to differentiate it from ClkActive. Expected impact is minimal. Named association calls will need to be updated with the new name. Procedures impacted: WaitForTransaction, WaitForTransactionOrIrq, WaitForBarrier(2), WaitForClock, WaitForLevel
   -  Moved CreateClock and CreateReset and friends from TbUtilPkg to ClockResetPkg

-  CoveragePkg: Coverage models with Coverage Weight of 0 print last and are hidden in the html by default
-  MemoryGenericPkg

   -  Throws Errors on AddrWidth > 40 and warnings if AddrWidth > 36.
   -  With the Error message, AddrWidth is truncated to 40 and the memory tries to operate.
   -  Note some simulators die even at AddrWidth = 36.
   -  Thanks to those reported tool crashes without warnings.

-  ClockResetPkg

   -  Updated CreateClock with Offset, ClkActive, and Enable . Clock starts with ClkActive value after the Offset. At startup it has the initial signal value if set, otherwise, not ClkActive (if Offset > 0 sec).
   -  Old version of create clock is available as OldCreateClock. Its startup behavior is slightly different.
   -  Old version of create clock is available as CreateClock when ClockResetVersion is set to 2024.05 or less in OsvvmSettingsLocal.tcl. New versions will not be available though.
   -  Added CreateJitterClock whose jitter is controlled by a coverage model
   -  Thanks to those who continued asking about an upgrade to CreateClock

-  ScoreboardPkg

   -  Made all generic functions impure
   -  Added scoreboards for unsigned, signed, and integer_vector (IntV)

-  ReportPkg

   -  Added TimeOut input to EndOfTestReports
   -  Added scoreboard reporting for: unsigned, signed, and integer_vector (IntV)

-  Scripts

   -  Include now has better error handling and restoration of state
   -  Updated support for Synopsys VCS, Cadence Xcelium, and added support for Siemens Visualizer
   -  Fixed DoWaves for Aldec. Fixed SetLogEnable.

-  AXI4

   -  Shortened AlertLogID Names to improve reports
   -  AxiStreamReceiver - Added OsvvmVcInit to support setting of WaitForGet before TReady is active.

-  UART: Minor updates to work around tool issues (Synopsys VCS and Cadence Xcelium).
-  Regressions run on

   -  GHDL 4.1.0. Latest release run on GitHub Actions
   -  NVC 1.13.0
   -  Aldec RivieraPRO 2023.10
   -  Aldec ActiveHDL 14
   -  Aldec VSimSA 14 (related to ActiveHDL)
   -  Siemens Questa 2024.02_1.
   -  Siemens Visualizer 2024.02_1 runs and individual tests pass, but hangs when running a large number of test cases
   -  Synopsys VCS V-2023.12-SP2
   -  Cadence Xcelium 2024.03 - passes osvvm library internal tests, but does not pass the OSVVM public tests (for OSVVM VC)
   -  Xilinx XSIM 2024.1 - passes osvvm library internal tests, but does not pass the OSVVM public tests (for OSVVM VC)

.. _CHANGELOG/2024.05b:

2024.05b
========

:Released: 2024-06-10
:Source: `Git tag 2024.05b <https://github.com/OSVVM/OsvvmLibraries/tree/2024.05b>`__

.. _CHANGELOG/2024.05a:

2024.05a
========

:Released: 2024-05-30
:Source: `Git tag 2024.05a <https://github.com/OSVVM/OsvvmLibraries/tree/2024.05a>`__

.. _CHANGELOG/2024.05:

2024.05
=======

:Released: 2024-05-28
:Source: `Git tag 2024.05 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.05>`__

.. _CHANGELOG/2024.03a:

2024.03a
========

:Released: 2024-05-02
:Source: `Git tag 2024.03a <https://github.com/OSVVM/OsvvmLibraries/tree/2024.03a>`__

.. _CHANGELOG/2024.03:

2024.03
=======

:Released: 2024-04-10
:Source: `Git tag 2024.03 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.03>`__


.. _CHANGELOG/2023:

2023
****

.. _CHANGELOG/2023.09a:

2023.09a
========

:Released: 2023-10-03
:Source: `Git tag 2023.09a <https://github.com/OSVVM/OsvvmLibraries/tree/2023.09a>`__

.. _CHANGELOG/2023.09:

2023.09
=======

:Released: 2023-09-29
:Source: `Git tag 2023.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.09>`__

.. _CHANGELOG/2023.07:

2023.07
=======

:Released: 2023-07-25
:Source: `Git tag 2023.07 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.07>`__

.. _CHANGELOG/2023.05:

2023.05
=======

:Released: 2023-06-11
:Source: `Git tag 2023.05 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.05>`__

.. _CHANGELOG/2023.04:

2023.04
=======

:Released: 2023-04-04
:Source: `Git tag 2023.04 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.04>`__

.. _CHANGELOG/2023.01:

2023.01
=======

:Released: 2023-02-04
:Source: `Git tag 2023.01 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.01>`__


.. _CHANGELOG/2022:

2022
****

.. _CHANGELOG/2022.12:

2022.12
=======

:Released: 2022-12-18
:Source: `Git tag 2022.12 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.12>`__

.. _CHANGELOG/2022.11:

2022.11
=======

:Released: 2022-11-25
:Source: `Git tag 2022.11 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.11>`__

.. _CHANGELOG/2022.10:

2022.10
=======

:Released: 2022-11-04
:Source: `Git tag 2022.10 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.10>`__

.. _CHANGELOG/2022.09:

2022.09
=======

:Released: 2022-09-11
:Source: `Git tag 2022.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.09>`__

.. _CHANGELOG/2022.08b:

2022.08b
========

:Released: 2022-08-20
:Source: `Git tag 2022.08b <https://github.com/OSVVM/OsvvmLibraries/tree/2022.08b>`__

.. _CHANGELOG/2022.08:

2022.08
=======

:Released: 2022-08-07
:Source: `Git tag 2022.08 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.08>`__

.. _CHANGELOG/2022.06a:

2022.06a
========

:Released: 2022-07-29
:Source: `Git tag 2022.06a <https://github.com/OSVVM/OsvvmLibraries/tree/2022.06a>`__

.. _CHANGELOG/2022.06:

2022.06
=======

:Released: 2022-07-21
:Source: `Git tag 2022.06 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.06>`__

.. _CHANGELOG/2022.05d:

2022.05d
========

:Released: 2022-06-20
:Source: `Git tag 2022.05d <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05d>`__

.. _CHANGELOG/2022.05c:

2022.05c
========

:Released: 2022-06-14
:Source: `Git tag 2022.05c <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05c>`__

.. _CHANGELOG/2022.05b:

2022.05b
========

:Released: 2022-06-09
:Source: `Git tag 2022.05b_ <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05b_>`__

.. _CHANGELOG/2022.05:

2022.05
=======

:Released: 2022-05-31
:Source: `Git tag 2022.05 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05>`__

.. _CHANGELOG/2022.04:

2022.04
=======

:Released: 2022-04-28
:Source: `Git tag 2022.04 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.04>`__

.. _CHANGELOG/2022.03:

2022.03
=======

:Released: 2022-04-03
:Source: `Git tag 2022.03 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.03>`__

.. _CHANGELOG/2022.02:

2022.02
=======

:Released: 2022-02-24
:Source: `Git tag 2022.02 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.02>`__

.. _CHANGELOG/2022.01:

2022.01
=======

:Released: 2022-01-16
:Source: `Git tag 2022.01 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.01>`__


.. _CHANGELOG/2021:

2021
****

.. _CHANGELOG/2021.12:

2021.12
=======

:Released: 2021-12-29
:Source: `Git tag 2021.12 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.12>`__

.. _CHANGELOG/2021.11:

2021.11
=======

:Released: 2021-12-03
:Source: `Git tag 2021.11 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.11>`__

.. _CHANGELOG/2021.10:

2021.10
=======

:Released: 2021-11-14
:Source: `Git tag 2021.10 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.10>`__

.. _CHANGELOG/2021.09:

2021.09
=======

:Released: 2021-09-24
:Source: `Git tag 2021.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.09>`__

.. _CHANGELOG/2021.07:

2021.07
=======

:Released: 2021-08-04
:Source: `Git tag 2021.07 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.07>`__

.. _CHANGELOG/2021.06:

2021.06
=======

:Released: 2021-06-30
:Source: `Git tag 2021.06 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.06>`__

.. _CHANGELOG/2021.02:

2021.02
=======

:Released: 2021-03-03
:Source: `Git tag 2021.02 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.02>`__


.. _CHANGELOG/2020:

2020
****

.. _CHANGELOG/2020.12a:

2020.12a
========

:Released: 2021-01-04
:Source: `Git tag 2020.12a <https://github.com/OSVVM/OsvvmLibraries/tree/2020.12a>`__

.. _CHANGELOG/2020.12:

2020.12
=======

:Released: 2020-12-31
:Source: `Git tag 2020.12 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.12>`__

.. _CHANGELOG/2020.10:

2020.10
=======

:Released: 2020-11-22
:Source: `Git tag 2020.10 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.10>`__

.. _CHANGELOG/2020.08a:

2020.08a
========

:Released: 2020-09-23
:Source: `Git tag 2020.08a <https://github.com/OSVVM/OsvvmLibraries/tree/2020.08a>`__

.. _CHANGELOG/2020.08:

2020.08
=======

:Released: 2020-09-01
:Source: `Git tag 2020.08 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.08>`__

.. _CHANGELOG/2020.07:

2020.07
=======

:Released: 2020-07-29
:Source: `Git tag 2020.07 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.07>`__

.. _CHANGELOG/2020.05:

2020.05
=======

:Released: 2020-05-19
:Source: `Git tag 2020.05 <https://github.com/OSVVM/OSVVM/tree/2020.05>`__


.. _CHANGELOG/2018:

2018
****

.. _CHANGELOG/2018.04:

2018.04
=======

:Released: 2018-04-10
:Source: `Git tag 2018.04 <https://github.com/OSVVM/OSVVM/tree/2018.04>`__


.. _CHANGELOG/2017:

2017
****

.. _CHANGELOG/2017.05:

2017.05
=======

:Released: 2017-06-12
:Source: `Git tag 2017.05 <https://github.com/OSVVM/OSVVM/tree/2017.05>`__


.. _CHANGELOG/2016:

2016
****

.. _CHANGELOG/2016.11:

2016.11
=======

:Released: 2016-12-08
:Source: `Git tag 2016.11 <https://github.com/OSVVM/OSVVM/tree/2016.11>`__

.. _CHANGELOG/2016.01:

2016.01
=======

:Released: 2016-01-26
:Source: `Git tag 2016.01_Release <https://github.com/OSVVM/OSVVM/tree/2016.01_Release>`__


.. _CHANGELOG/2015:

2015
****

.. _CHANGELOG/2015.06:

2015.06
=======

:Source: `Git tag 2015.06 <https://github.com/OSVVM/OSVVM/tree/2015.06>`__

.. _CHANGELOG/2015.03:

2015.03
=======

:Source: `Git tag 2015.03 <https://github.com/OSVVM/OSVVM/tree/2015.03>`__


.. _CHANGELOG/2014:

2014
****

.. _CHANGELOG/2014.07a:

2014.07a
========

:Source: `Git tag 2014.07a <https://github.com/OSVVM/OSVVM/tree/2014.07a>`__

.. _CHANGELOG/2014.01:

2014.01
=======

:Source: `Git tag 2014.01 <https://github.com/OSVVM/OSVVM/tree/2014.01>`__


.. _CHANGELOG/2013:

2013
****

.. _CHANGELOG/2013.05:

2013.05
=======

:Source: `Git tag 2013.05 <https://github.com/OSVVM/OSVVM/tree/2013.05>`__
