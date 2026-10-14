.. _RELEASES:

Release History
###############

Most releases are announced on the `OSVVM Blog <https://osvvm.org/blog>`__; each version below links to its
announcement and summarizes it, or shows the description of its GitHub release page. A version without either is
documented only in the ``CHANGELOG.md`` files of OSVVM's repositories, linked from OsvvmLibraries'
`CHANGELOG.md <https://github.com/OSVVM/OsvvmLibraries/blob/main/CHANGELOG.md>`__.

Since 2020.07, a release of OSVVM is a release of `OsvvmLibraries <https://github.com/OSVVM/OsvvmLibraries>`__, which
collects all OSVVM repositories as Git submodules. Earlier releases are releases of the OSVVM utility library
(`OSVVM/OSVVM <https://github.com/OSVVM/OSVVM>`__).


.. _RELEASES/2026:

2026
****

.. _RELEASES/2026.09:

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

.. _RELEASES/2026.08:

2026.08
=======

:Released: 2026-09-10
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2026.08>`__

Announced in `OSVVM 2026.08 <https://osvvm.org/archives/2994>`__ (2026-09-10).

* Requirements tracking: additional settings in ``OsvvmSettingsPkg``.
* Multi-line ``Alert``, ``Log``, ``LogHeader``, ``PrintLine`` and ``PrintHeader`` with line wrapping either at the end
  of the Alert/Log prefix or indented from the left column.
* New settings methodology: ``OsvvmSettingsPkg_local.vset`` contains only the changed values (``NAME: Value``); all
  other settings keep their defaults, so new settings no longer break local settings files.
* ``MemoryPkg`` storage policy selectable at compile time via Tcl variable ``$::osvvm::UseMemoryPkg01``; instances
  ``MemoryPkg_UX01``, ``MemoryPkg_01`` and ``MemoryPkg`` (default policy ``UX01``, used by ``Axi4Memory``).
* Storage policies renamed: ``X`` → ``UX01``, ``NoX`` → ``01``.
* New ``format`` function: simulator-independent alternative to ``to_string`` for ``integer`` (prints
  ``integer'high``/``integer'low``), ``real`` (fixed point or exponent, configurable digits) and ``time`` (configurable
  unit and fraction digits).
* ``SetExpectedAlertCount`` and ``IncrementExpectedAlertCount`` can be called at any time; replaces passing negative
  values via ``ExternalErrors`` of ``EndOfTestReports``.
* Scripts: ``ExpectedStatus`` (e.g. a test expected to fail) and ``KnownStatus`` (known, currently unfixable failures);
  build reports add "Untracked Failures" and "Changes in Tracked Test Cases".
* New test status ``STOPLIMIT`` when a test ends because an alert stop count was reached.
* Transcript files opened with ``TranscriptOpen`` are HTML'ized (errors and failures in red); with mirroring off, the
  transcript is embedded into the HTML log via frame (requires ``OSVVM_PREFIX_X_MARKS_THE_SPOT: "%%x "`` in
  ``OsvvmSettingsPkg_local.vset``).
* **Breaking:** Alert/Log spacing changed and time is printed with a decimal point; transcript comparisons fail unless
  ``ALERT_LOG_IGNORE_SPACES``, ``ALERT_LOG_IGNORE_EMPTY_LINES`` and ``ALERT_LOG_DIGITS_FOR_TIME_FRACTION`` are set.

.. _RELEASES/2026.05:

2026.05
=======

:Released: 2026-06-07
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2026.05>`__

Announced in `OSVVM 2026.05 <https://osvvm.org/archives/2933>`__ (2026-06-06), with follow-ups `2026.05 and
SetAxi4Options/GetAxi4Options <https://osvvm.org/archives/2944>`__ (2026-06-17) and `2026.05 and AXI4Memory
<https://osvvm.org/archives/2948>`__ (2026-06-22).

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

.. _RELEASES/2026.01:

2026.01
=======

:Released: 2026-03-01
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2026.01>`__

Announced in `OSVVM 2026.01 <https://osvvm.org/archives/2882>`__ (2026-03-05).

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


.. _RELEASES/2025:

2025
****

.. _RELEASES/2025.06a:

2025.06a
========

:Released: 2025-09-21
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.06a>`__

.. _RELEASES/2025.06:

2025.06
=======

:Released: 2025-07-19
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.06>`__

Announced in `OSVVM 2025.06 <https://osvvm.org/archives/2756>`__ (2025-07-22).

.. attention::

   The scripts in this release have some issues. Please use :ref:`2025.06a <RELEASES/2025.06a>` instead.

* Build output written to a build directory, ``<DirName>_<ScriptName>`` by default or set via ``BuildName`` (on the
  ``build`` command line or inside the ``*.pro`` file).
* New Index of Builds: HTML summary of all previously run builds.
* Scripts: new ``CreateTestCaseCommonPkg`` generates a package with test suite path and name constants, e.g.
  ``PATH_TO_VALIDATED_RESULTS``.
* Scripts: new ``RunAllTests`` runs all tests matching a name pattern (e.g. ``TbUart_``).
* Questa: ``OSVVM_TOOL`` = ``Questa`` selects the new ``vopt`` + ``vsim`` flow (``VendorScripts_Questa.tcl``, default
  for Questa 2025.2 and newer); ``Siemens`` selects the old ``vsim`` flow (``VendorScripts_Siemens.tcl``).
* ``WaitForClock``/``WaitForTransaction``: ``ClkActive`` accepts ``'1'`` (rising, default), ``'0'`` (falling) and
  ``'-'`` (any edge).
* VC test cases use ``CreateTestCaseCommonPkg``, ``TranscriptOpen`` without parameters and ``EndOfTestReports`` with
  ``TIMEOUT`` and ``NOCHECKS`` detection.

.. _RELEASES/2025.04:

2025.04
=======

:Released: 2025-05-06
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.04>`__

.. _RELEASES/2025.02:

2025.02
=======

:Released: 2025-02-26
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2025.02>`__

Announced in `XSIM Loves OSVVM <https://osvvm.org/archives/2624>`__ (2025-02-24).

* AMD/Xilinx XSIM 2024.2 support for the OSVVM libraries and scripts.
* ``MemoryPkg`` and ``TextUtilPkg``: updates for XSIM (``MemoryPkg`` crashed XSIM 2024.2).
* Scripts: reports work with XSIM 2024.2 (Tcl 8.6); proper log files replace the XSIM 2023.2 workaround.
* AXI4 Full and AXI4 Lite VCs: work with XSIM without Xilinx-specific variants.
* Known XSIM limitations: ``InitSeed`` with ``time`` (``std.env.resolution_limit`` returns 0), resolution functions for
  ``real``/``time``, and VCs with ``time``-valued options (e.g. UART baud rate).


.. _RELEASES/2024:

2024
****

.. _RELEASES/2024.11:

2024.11
=======

:Released: 2024-12-07
:Source: `Git tag 2024.11 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.11>`__

.. _RELEASES/2024.09:

2024.09
=======

:Released: 2024-09-04
:Source: `Git tag 2024.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.09>`__

.. _RELEASES/2024.07:

2024.07
=======

:Released: 2024-08-01
:Source: `GitHub release <https://github.com/OSVVM/OsvvmLibraries/releases/tag/2024.07>`__

Announced in `OSVVM release 2024.07 + Conference Trip Report <https://osvvm.org/archives/2546>`__ (2024-07-27).

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

.. _RELEASES/2024.05b:

2024.05b
========

:Released: 2024-06-10
:Source: `Git tag 2024.05b <https://github.com/OSVVM/OsvvmLibraries/tree/2024.05b>`__

Mentioned in `OSVVM 2024.05 Release <https://osvvm.org/archives/2428>`__ (2024-05-30).

* Fixed missing hierarchy in the alert report.

.. _RELEASES/2024.05a:

2024.05a
========

:Released: 2024-05-30
:Source: `Git tag 2024.05a <https://github.com/OSVVM/OsvvmLibraries/tree/2024.05a>`__

Mentioned in `OSVVM 2024.05 Release <https://osvvm.org/archives/2428>`__ (2024-05-30).

* Fixed links to code coverage reports.

.. _RELEASES/2024.05:

2024.05
=======

:Released: 2024-05-28
:Source: `Git tag 2024.05 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.05>`__

Announced in `OSVVM 2024.05 Release <https://osvvm.org/archives/2428>`__ (2024-05-30).

* Reports: more compact output format; report generation code refactored.
* SPI: new ``SpiController`` and ``SpiPeripheral`` VCs and a detailed SPI test suite.
* ``RandomPkg`` fix: the ``max >= min`` check was reported twice.
* ``NameStorePkg``: singleton procedures pass all parameters to the protected type methods.

.. _RELEASES/2024.03a:

2024.03a
========

:Released: 2024-05-02
:Source: `Git tag 2024.03a <https://github.com/OSVVM/OsvvmLibraries/tree/2024.03a>`__

Mentioned in `OSVVM 2024.03 Release <https://osvvm.org/archives/2365>`__ (2024-04-11).

* ``EndSimulation`` no longer closes waveform windows on Tcl errors (except with ``SetInteractiveMode``).

.. _RELEASES/2024.03:

2024.03
=======

:Released: 2024-04-10
:Source: `Git tag 2024.03 <https://github.com/OSVVM/OsvvmLibraries/tree/2024.03>`__

Announced in `OSVVM 2024.03 Release <https://osvvm.org/archives/2365>`__ (2024-04-11).

* Settings via deferred constants in ``OsvvmSettingsPkg`` and ``OsvvmScriptSettingsPkg``; package bodies are selectable
  and compiled last.
* ``RANDOM_USE_NEW_SEED_METHODS``: ``InitSeed`` uses new seed methods by default (randomization results change).
* ``COVERAGE_DEFAULT_WEIGHT_MODE`` changed from ``AT_LEAST`` to ``REMAIN`` (randomization results change).
* **Breaking:** ``ALERT_LOG_JUSTIFY_ENABLE``, ``ALERT_LOG_WRITE_TIME_FIRST`` and ``ALERT_LOG_TIME_JUSTIFY_AMOUNT``
  change the default Alert/Log formatting.
* **Breaking:** ``SCOREBOARD_YAML_IS_BASE_FILE_NAME``: ``WriteScoreboardYaml`` takes a base name instead of a file name.
* String-valued options of ``SetAlertLogOptions`` and ``SetReportOptions`` deprecated (ignored with warning); set via
  ``OsvvmSettingsPkg``. ``SetOsvvmGlobalOptions`` removed.
* ``OsvvmSettingsLocal.tcl`` replaces ``LocalScriptsDefaults.tcl`` (still read if the new file is absent);
  ``OsvvmVersionCompatibility "2023"`` restores the previous defaults; new ``SettingsAreRelativeToSimulationDirectory``
  and ``OsvvmSettingsSubDirectory``.
* AMD/Xilinx XSIM 2023.2: packages updated, improved script support, ``osvvm.pro`` selects XSIM-specific packages; most
  VC tests still fail (reported to AMD).
* New VCs: SPI (Guy Eschemann) and VideoBus (Louis Adriaens).
* ``ResizePkg``: ``SafeResize(AlertLogID, …)`` with improved message; used by all OSVVM VCs.
* VCs use the ``ModelParametersIDType`` singleton in the transaction record instead of a shared
  ``ModelParametersPType``.
* ``AlertLogPkg``: new ``AffirmIfTranscriptsMatch``; ``AffirmIfEqual`` for ``boolean``.
* ``RandomPkg``: ``SetRandomSalt`` overload returning its parameter type.
* ``MemoryPkg``: aliases ``MemWrite`` and ``MemRead``.
* Scripts: ``OpenBuildHtml`` (Siemens and Aldec).
* ``FifoFillPkg_slv``: ``PopBurstVector`` returns a count for ``slv_vector``/``integer_vector``.
* ``AddressBusTransactionPkg``: ``SetModelOptions`` for ``time``.
* Fix: ``NewID`` name collisions between ``MemoryPkg``, ``CoveragePkg`` and ``ScoreboardPkg`` crashed
  ``EndOfTestReports``.
* Fix: scripts close files left open after a simulator error.


.. _RELEASES/2023:

2023
****

.. _RELEASES/2023.09a:

2023.09a
========

:Released: 2023-10-03
:Source: `Git tag 2023.09a <https://github.com/OSVVM/OsvvmLibraries/tree/2023.09a>`__

Announced in `OSVVM 2023.09a Release <https://osvvm.org/archives/2304>`__ (2023-10-10); it also announces 2023.09.

* Fixed co-simulation issues introduced in 2023.09.

.. _RELEASES/2023.09:

2023.09
=======

:Released: 2023-09-29
:Source: `Git tag 2023.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.09>`__

Announced together with 2023.09a in `OSVVM 2023.09a Release <https://osvvm.org/archives/2304>`__ (2023-10-10).

* Scripts: ``build``/``include`` detect missing files and paths early with better messages.
* Scripts: ``$::ARGV``/``$::ARGC`` (array) and ``$::argv``/``$::argc``/``$::argv0`` (list).
* New ``OsvvmSettingsPkg`` with deferred constants providing defaults for ``InitSeed`` seed method, Alert/Log
  justification and time-first printing, and the ``CoveragePkg`` weight mode.
* ``AlertLogPkg``, ``ScoreboardPkg``: updated Alert/Log message printing.
* New ``ModelParametersSingletonPkg``.
* Stream and Address Bus MIT: ``ModelParametersIdType``, ``ClassifyUnimplementedOperation`` and ``EXTEND_*`` operations
  for VC-specific options.
* Stream MIT: ``SendAndGet`` and ``SendAndGetBurst``.
* ``ScoreboardGenericPkg``: ``WriteScoreboardYaml`` parameters for simpler names.

.. _RELEASES/2023.07:

2023.07
=======

:Released: 2023-07-25
:Source: `Git tag 2023.07 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.07>`__

Announced in `OSVVM 2023.07 Release <https://osvvm.org/archives/2257>`__ (2023-07-26).

* ``AlertLogPkg``: new ``WriteRequirementsYaml``; called by ``EndOfTestReports`` when ``GotRequirements`` is set.
* Scripts: test case requirements merged into test suite requirements (HTML) and build requirements (HTML and CSV).
* Scripts: updated Build Summary and Test Case Summary HTML.
* ``DpRam``: burst support (Louis Adriaens).

.. _RELEASES/2023.05:

2023.05
=======

:Released: 2023-06-11
:Source: `Git tag 2023.05 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.05>`__

Announced in `OSVVM 2023.05 Release <https://osvvm.org/archives/2238>`__ (2023-06-12).

* Address Bus MIT: split transactions for managers; support for subordinates/responders.
* Stream MIT: test data pattern generation and data check methods.
* Randomized ready/valid delays: ``AxiStreamTransmitter``/``AxiStreamReceiver`` (``TValid``/``TReady``) and
  ``Axi4Manager``/``Axi4Memory``/``Axi4Subordinate`` (``xValid``/``xReady``).
* New ``DelayCoveragePkg`` for cycle-based delay randomization (part of ``OsvvmContext``,
  ``DelayCoveragePkg_user_guide.pdf``).
* ``StreamTransactionPkg``/``AddressBusTransactionPkg``: ``SetUseRandomDelays``, ``SetDelayCoverageID``,
  ``GetDelayCoverageID``.
* ``StreamTransactionPkg``: ``integer_vector`` overloads of ``SendBurstVector[Async]`` and ``[Try]CheckBurstVector``.
* ``FifoFillPkg_slv``: ``CheckBurstFifo``.
* ``CoveragePkg``: ``InitSeed`` includes the parent ID name (randomization results change); new ``IsNotCovered``.
* ``AlertLogPkg`` fix: YAML omits items with ``ReportMode`` ``DISABLED``.
* ``ScoreboardGenericPkg``: failed pop from an empty FIFO/scoreboard prints the tag.
* Ethernet fix: receiver uses repeat-until loop (stale value in first check).
* ``DpRamController_Blocking`` fix: correct address logged for reads.

.. _RELEASES/2023.04:

2023.04
=======

:Released: 2023-04-04
:Source: `Git tag 2023.04 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.04>`__

Announced in `OSVVM 2023.04 Release <https://osvvm.org/archives/2186>`__ (2023-04-08).

* Co-simulation supports streaming and address bus interfaces.
* ``OsvvmScriptSettings.vhd`` holds deferred constants; generated body ``OsvvmScriptSettings_Generated.vhd`` is
  git-ignored.
* Interrupt handler supports an array of interrupts.
* Fixes: Ethernet enables, ``Peek`` with tag, UART false break check in no-parity mode.

.. _RELEASES/2023.01:

2023.01
=======

:Released: 2023-02-04
:Source: `Git tag 2023.01 <https://github.com/OSVVM/OsvvmLibraries/tree/2023.01>`__

Announced in `OSVVM 2023.01 Release <https://osvvm.org/archives/2149>`__ (2023-02-05).

* New co-simulation environment: C++ software runs against any VC implementing the Address Bus MIT.
* Generated ``OsvvmScriptSettings.vhd`` with ``OSVVM_OUTPUT_DIRECTORY`` (referenced by ``OsvvmContext``); set via Tcl
  ``OsvvmTemporaryOutputDirectory`` (default ``CurrentSimulationDirectory``, requires rebuilding the libraries).
* New ``InterruptGeneratorBit.vhd`` (interrupts from an external device) and ``CoSimInterruptHandler.vhd``.
* ``InterruptHandler.vhd`` works in co-simulation environments without interrupt switching.
* PassThru VCs for AXI4, AXI4 Lite and AXI Stream as placeholder DUT (CoSim repository).


.. _RELEASES/2022:

2022
****

.. _RELEASES/2022.12:

2022.12
=======

:Released: 2022-12-18
:Source: `Git tag 2022.12 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.12>`__

Announced in `OSVVM 2022.12 Release <https://osvvm.org/archives/2133>`__ (2022-12-23).

* ``StartUp.tcl``: environment variable ``OSVVM_TOOL`` selects the simulator when none is detected (previously GHDL);
  ``OSVVM_TOOL=<Name>`` requires ``VendorScripts_<Name>.tcl``.
* Updated scripts for Synopsys VCS and Cadence Xcelium.
* Build log starts as ``OsvvmBuild.log`` in the simulation directory and is moved to ``logs/<BuildName>.log``.
* Top-level generics on ``simulate`` and ``RunTest``; generic values are part of transcript and test case report file
  names; HTML log links to tests with generics.
* Report generation depends only on static OSVVM information: ``Report2Html``, ``Report2Junit``, ``Simulate2Html``,
  ``Log2Osvvm`` run standalone; ``StartUpReports.tcl`` for plain ``tclsh``.
* ``SetTranscriptType none`` disables links to HTML logs and coverage reports.

.. _RELEASES/2022.11:

2022.11
=======

:Released: 2022-11-25
:Source: `Git tag 2022.11 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.11>`__

Announced in `OSVVM 2022.11 Release <https://osvvm.org/archives/2103>`__ (2022-11-26).

* Transactions on arrays of VCs: ``StreamTransactionArrayPkg``, ``AddressBusTransactionArrayPkg``,
  ``StreamRecArrayType``, ``AddressBusRecArrayType``; updated multiple-VC example.
* Interrupt handler example moved to ``Common/TbInterrupt``; new InterruptHandler user guide.
* Scripts: scoreboard HTML reports for user scoreboards written with ``WriteScoreboardYaml`` (``*_sb_*.yml``).
* ``AlertLogPkg``: ``SetTestName`` (``SetAlertLogName`` deprecated, kept as alias).
* Scripts: ``TestName`` replaces ``TestCase`` (kept as wrapper).
* Default search mode of scoreboards/FIFOs, coverage and memory models changed to ``PRIVATE_NAME``; the 2022.02 default
  ``NAME_AND_PARENT_ELSE_PRIVATE`` caused unintended sharing in ``for generate``.

.. _RELEASES/2022.10:

2022.10
=======

:Released: 2022-11-04
:Source: `Git tag 2022.10 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.10>`__

Announced in `OSVVM 2022.10 Release <https://osvvm.org/archives/2086>`__ (2022-11-06).

* New Ethernet PHY and MAC VCs for GMII, RGMII, MII and RMII.
* New ``InterruptHandler.vhd`` for VCs with Address Bus MIT; ``InterruptHandlerComponentPkg`` via
  ``OsvvmCommonContext``.
* ``RandomPkg``: ``SetRandomSalt`` (``string`` or ``integer``) and ``GetRandomSalt`` offset all ``InitSeed``
  calculations.
* Multiple-UART demo (``UART/testbench_multiple_uarts``) using ``for generate`` with one ``CentralTestProc``.
* Support for the NVC simulator.
* ``AlertLogPkg``: ``SetTestName`` replaces ``SetAlertLogName`` (alias kept).
* Scripts: errors in red in HTML logs; new ``DoWaves``, ``ChangeWorkingDirectory`` and ``JoinWorkingDirectory``.
* Scripts: errors in ``wave.do`` caught and ignored (``CallbackOnError_WaveDo``); ``wave.do`` not run in batch mode.
* GHDL: analyze/simulate output captured in log files; generics on ``simulate`` fixed.
* Questa: workaround for ``PushBurstRandom``/``CheckBurstRandom`` failures; Questa-Intel notes on stale ``modelsim.ini``
  mapping and ``vsim`` vs. ``questasim`` logs.
* ``.gitmodules`` uses relative submodule URLs.

.. _RELEASES/2022.09:

2022.09
=======

:Released: 2022-09-11
:Source: `Git tag 2022.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.09>`__

Announced in `Announcing Release 2022.09+ <https://osvvm.org/archives/2055>`__ (2022-09-15), details in `Scripts:
Generics, Waveform Files, and Run Length <https://osvvm.org/archives/2075>`__.

* Scripts CI signaling: ``FailOnBuildErrors`` (default ``true``), ``FailOnReportErrors``, ``FailOnTestCaseErrors``
  (default ``false``), ``AnalyzeErrorStopCount``, ``SimulateErrorStopCount`` (default ``0``).
* ``MemoryPkg``: unlimited data width (31-bit limit removed) with bitwise ``X`` fidelity; new ``MemoryPkg_NoX`` (``X`` →
  ``0``, less storage) and ``MemoryPkg_orig`` (31-bit, original ``X`` policy).
* Reports: scoreboards as rows of one table; text mini build summary; errors in red in HTML transcripts.
* Scripts: simulate with two top-level design units.
* Scripts: ``SetExtendedElaborateOptions``/``SetExtendedRunOptions`` (GHDL); ``SetSaveWaves true`` writes
  ``<Testbench>.ghw``.
* Scripts: ``generic`` without quoting, e.g. ``simulate Tb [generic WIDTH 8]``.
* Scripts: waveform/Tcl files run during ``simulate`` searched by tool, library unit and test case name.
* Scripts: ``CallbackBefore_<Cmd>``/``CallbackAfter_<Cmd>`` for ``library``, ``build``, ``include``, ``analyze``,
  ``simulate``; error handling in ``CallbackOnError_<Cmd>``.
* Scripts: Verilog resource libraries added automatically during ``analyze``.

.. _RELEASES/2022.08b:

2022.08b
========

:Released: 2022-08-20
:Source: `Git tag 2022.08b <https://github.com/OSVVM/OsvvmLibraries/tree/2022.08b>`__

.. _RELEASES/2022.08:

2022.08
=======

:Released: 2022-08-07
:Source: `Git tag 2022.08 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.08>`__

.. _RELEASES/2022.06a:

2022.06a
========

:Released: 2022-07-29
:Source: `Git tag 2022.06a <https://github.com/OSVVM/OsvvmLibraries/tree/2022.06a>`__

.. _RELEASES/2022.06:

2022.06
=======

:Released: 2022-07-21
:Source: `Git tag 2022.06 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.06>`__

Announced in `Announcing OSVVM Release 2022.06 <https://osvvm.org/archives/2025>`__ (2022-07-23).

* Scripts: one-line text build summary at the end of a build.
* Scripts: files closed when report generation fails.
* Scripts: text log always created; HTML transcript optional (``SetTranscriptType html``); simulator transcript free of
  HTML tags.
* Scripts: ``CreateSimScripts true`` writes the issued simulator commands to a ``.tcl`` file (for vendor bug reports).
* Scripts (2022.05/2022.06): all analyze/simulate errors caught; ``FailOnBuildErrors``, ``FailOnReportErrors``,
  ``FailOnTestCaseErrors``, ``AnalyzeErrorStopCount``, ``SimulateErrorStopCount``.
* ``AlertLogPkg``: new output formatter, e.g. time first, via ``SetAlertLogOptions`` and ``SetAlertLogJustify`` (all
  AlertLogIDs must exist before ``SetAlertLogJustify``).
* ``CoveragePkg``: YAML reports ``PASSED``/``FAILED``; ``SetReportOptions(WritePassFail => ENABLED)`` shows it in the
  HTML test case report.

.. _RELEASES/2022.05d:

2022.05d
========

:Released: 2022-06-20
:Source: `Git tag 2022.05d <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05d>`__

Mentioned in `Announcing OSVVM Release 2022.06 <https://osvvm.org/archives/2025>`__ (2022-07-23).

* Scripts: generics on ``simulate``, e.g. ``simulate Tb [generic a 5]`` (Riviera-PRO, Active-HDL, ModelSim, QuestaSim).

.. _RELEASES/2022.05c:

2022.05c
========

:Released: 2022-06-14
:Source: `Git tag 2022.05c <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05c>`__

.. _RELEASES/2022.05b:

2022.05b
========

:Released: 2022-06-09
:Source: `Git tag 2022.05b_ <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05b_>`__

.. _RELEASES/2022.05a:

2022.05a
========

Announced in `Release 2022.05a <https://osvvm.org/archives/2005>`__ (2022-06-06).

* Fix: HTML transcripts from 2022.05 required a newer Tcl version; restores compatibility with older tools such as
  Riviera-PRO 2018.02.

.. _RELEASES/2022.05:

2022.05
=======

:Released: 2022-05-31
:Source: `Git tag 2022.05 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.05>`__

Announced in `Announcing OSVVM Release 2022.05+ <https://osvvm.org/archives/1982>`__ (2022-06-01).

* Scripts: improved error handling; HTML transcripts also for non-GUI tools such as GHDL.
* Build summary report includes analyze and simulate errors and links to simulator transcripts (log or HTML).
* Simplified code coverage handling: run, save, merge and report.
* All VCs define ``MODEL_ID_NAME``; FIFOs use search mode ``PRIVATE`` (unique FIFOs in generate loops).

.. _RELEASES/2022.04:

2022.04
=======

:Released: 2022-04-28
:Source: `Git tag 2022.04 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.04>`__

Announced in `Announcing OSVVM Release 2022.05+ <https://osvvm.org/archives/1982>`__ (2022-06-01).

* New https://osvvm.github.io site and ``OSVVM_Overview.pdf``; updated READMEs.
* ``Axi4Memory``: delay between words of read burst data.

.. _RELEASES/2022.03:

2022.03
=======

:Released: 2022-04-03
:Source: `Git tag 2022.03 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.03>`__

Announced in `Announcing OSVVM Release 2022.05+ <https://osvvm.org/archives/1982>`__ (2022-06-01).

* New documents: OSVVM Structured Testbench Framework and Verification Component Developer's Guide.
* Build and test case reports link to transcript files opened with ``TranscriptOpen``.
* AXI4 Lite: rebased on the AXI4 Full implementation; all GHDL issues resolved.
* AXI Stream: one burst may span multiple ``SendBurst`` calls.
* ``TbUtilPkg``: ``EdgeRose``, ``EdgeFell``, ``EdgeActive``, ``FindRisingEdge``, ``FindFallingEdge``,
  ``FindActiveEdge``.

.. _RELEASES/2022.02:

2022.02
=======

:Released: 2022-02-24
:Source: `Git tag 2022.02 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.02>`__

Announced in `Announcing OSVVM Release 2022.02 <https://osvvm.org/archives/1936>`__ (2022-02-25).

* Simulator transcripts written as HTML or text.
* Test case reports link to the test's section in the HTML transcript.
* Scoreboard reports in the test case reports.
* Scripts: optional code coverage collection, merging and HTML reporting, linked from the build report.

.. _RELEASES/2022.01:

2022.01
=======

:Released: 2022-01-16
:Source: `Git tag 2022.01 <https://github.com/OSVVM/OsvvmLibraries/tree/2022.01>`__


.. _RELEASES/2021:

2021
****

.. _RELEASES/2021.12:

2021.12
=======

:Released: 2021-12-29
:Source: `Git tag 2021.12 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.12>`__

.. _RELEASES/2021.11:

2021.11
=======

:Released: 2021-12-03
:Source: `Git tag 2021.11 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.11>`__

.. _RELEASES/2021.10:

2021.10
=======

:Released: 2021-11-14
:Source: `Git tag 2021.10 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.10>`__

Announced in `OSVVM 2021.10: Build Summary Reports <https://osvvm.org/archives/1879>`__ (2021-11-18) and `OSVVM 2021.10:
Functional Coverage Reports <https://osvvm.org/archives/1897>`__ (2021-12-07).

* HTML Build Summary Report: hierarchical build, test suite and test case status.
* HTML test case reports: alerts per AlertLogID and functional coverage (bins shown, settings collapsed).
* ``EndOfTestReports`` replaces ``ReportAlerts``.
* YAML intermediate format replaces the CSV test summaries (``WriteTestSummary``, ``ReadTestSummaries``,
  ``ReportTestSummaries`` from 2020.05).
* Scripts: ``TestSuite`` and ``TestCase``; ``RunTest`` combines ``analyze`` and ``simulate``.
* New failure ``NAME_MISMATCH`` when ``TestCase`` and ``SetAlertLogName`` names differ; a test without YAML status is
  reported as failed.

.. _RELEASES/2021.09:

2021.09
=======

:Released: 2021-09-24
:Source: `Git tag 2021.09 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.09>`__

.. _RELEASES/2021.07:

2021.07
=======

:Released: 2021-08-04
:Source: `Git tag 2021.07 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.07>`__

.. _RELEASES/2021.06:

2021.06
=======

:Released: 2021-06-30
:Source: `Git tag 2021.06 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.06>`__

.. _RELEASES/2021.02:

2021.02
=======

:Released: 2021-03-03
:Source: `Git tag 2021.02 <https://github.com/OSVVM/OsvvmLibraries/tree/2021.02>`__

Announced in `Announcing OSVVM Release 2021.02 <https://osvvm.org/archives/1799>`__ (2021-03-04).

* Scripts: improved Active-HDL and GHDL support.
* AXI4 and AXI Stream VCs: valid delays.
* Multiple driver detection.
* ``RequestTransaction``/``WaitForTransaction`` for ``integer``.


.. _RELEASES/2020:

2020
****

.. _RELEASES/2020.12a:

2020.12a
========

:Released: 2021-01-04
:Source: `Git tag 2020.12a <https://github.com/OSVVM/OsvvmLibraries/tree/2020.12a>`__

.. _RELEASES/2020.12:

2020.12
=======

:Released: 2020-12-31
:Source: `Git tag 2020.12 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.12>`__

Announced in `OSVVM 2020.12: Virtual Transaction Interfaces <https://osvvm.org/archives/1785>`__ (2021-01-11).

* Virtual Transaction Interfaces (entity suffix ``Vti``): ``Axi4MasterVti``, ``Axi4ResponderVti_Transactor``,
  ``Axi4MemoryVti``, ``AxiStreamTransmitterVti``, ``AxiStreamReceiverVti``.
* Test harness and test sequencer run the same test cases against VTI components.

.. _RELEASES/2020.10:

2020.10
=======

:Released: 2020-11-22
:Source: `Git tag 2020.10 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.10>`__

Announced in `OSVVM 2020.10: AXI Stream: Bursting + Full Signaling Support <https://osvvm.org/archives/1745>`__
(2020-11-28).

* ``AxiStreamTransmitter``/``AxiStreamReceiver``: complete AXI Stream signaling and full Stream MIT support.
* Bursting via burst FIFOs; direct or algorithmic ``TLast`` control for single-word transfers.
* ``TStrb``/``TKeep`` for single words and bursts, including sparse streams.
* ``TID``, ``TDest``, ``TUser`` from VC defaults or per transaction; ``TID``/``TDest`` constant within a burst,
  ``TUser`` per transfer or per word.

.. _RELEASES/2020.08a:

2020.08a
========

:Released: 2020-09-23
:Source: `Git tag 2020.08a <https://github.com/OSVVM/OsvvmLibraries/tree/2020.08a>`__

.. _RELEASES/2020.08:

2020.08
=======

:Released: 2020-09-01
:Source: `Git tag 2020.08 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.08>`__

Announced in `OSVVM 2020.08: Requirements Tracking + Updated Scripting <https://osvvm.org/archives/1700>`__
(2020-09-03).

* Requirements tracking in ``AlertLogPkg``: ``PASSED``/``FAILED`` via ``AffirmIf``, also by requirement name.
* ``ReportRequirements`` (test fails on unmet requirements by default), ``WriteRequirements`` (CSV),
  ``ReadRequirements`` (merge runs), ``ReadSpecification`` (pre-fill requirement bins).
* Scripts: simulator-independent Tcl API (executable scripts) for Aldec and Mentor simulators.

.. _RELEASES/2020.07:

2020.07
=======

:Released: 2020-07-29
:Source: `Git tag 2020.07 <https://github.com/OSVVM/OsvvmLibraries/tree/2020.07>`__

Announced in `Announcing OSVVM 2020.07: AXI4 + Model Independent Transactions <https://osvvm.org/archives/1688>`__
(2020-07-31).

* New AXI4 Full VCs (master, responder, memory) with bursting.
* AXI4 Lite, AXI Stream and UART VCs use Model Independent Transactions (address bus and stream).
* AXI4/AXI4 Lite: write address, write data, write response, read address and read data channels operate independently.

.. _RELEASES/2020.05:

2020.05
=======

:Released: 2020-05-19
:Source: `Git tag 2020.05 <https://github.com/OSVVM/OSVVM/tree/2020.05>`__

Announced in `Announcing OSVVM 2020.05 <https://osvvm.org/archives/1647>`__ (2020-05-20).

* ``AlertLogPkg`` tracks ``PASSED`` conditions; ``ReportAlerts`` and ``%%`` lines show passed counts per AlertLogID.
* ``PASSED`` goals grouped under ``REQUIREMENT_ALERTLOG_ID``.
* ``SetAlertLogOptions(PrintPassed => DISABLED)`` suppresses per-model passed counts.
* Updates to ``ScoreboardGenericPkg`` and ``CoveragePkg`` (details in the release notes).


.. _RELEASES/2018:

2018
****

.. _RELEASES/2018.04:

2018.04
=======

:Released: 2018-04-10
:Source: `Git tag 2018.04 <https://github.com/OSVVM/OSVVM/tree/2018.04>`__


.. _RELEASES/2017:

2017
****

.. _RELEASES/2017.05:

2017.05
=======

:Released: 2017-06-12
:Source: `Git tag 2017.05 <https://github.com/OSVVM/OSVVM/tree/2017.05>`__


.. _RELEASES/2016:

2016
****

.. _RELEASES/2016.11a:

2016.11a
========

Announced in `Announcing OSVVM 2016.11a <https://osvvm.org/archives/1293>`__ (2016-12-16).

* ``VendorCovApiPkg_Aldec.vhd``: attribute bug fixed; works with Riviera-PRO 2016.10.

.. _RELEASES/2016.11:

2016.11
=======

:Released: 2016-12-08
:Source: `Git tag 2016.11 <https://github.com/OSVVM/OSVVM/tree/2016.11>`__

Announced in `Announcing OSVVM 2016.11 <https://osvvm.org/archives/1251>`__ (2016-12-09).

* New ``VendorCovApiPkg``: forwards coverage model creation and coverage recording to the simulator's coverage database
  (``VendorCovApiPkg_Aldec.vhd``).

.. _RELEASES/2016.01:

2016.01
=======

:Released: 2016-01-26
:Source: `Git tag 2016.01_Release <https://github.com/OSVVM/OSVVM/tree/2016.01_Release>`__

Announced in `Announcing OSVVM release 2016.01 <https://osvvm.org/archives/1101>`__ (2016-01-26).

* ``AlertLogPkg`` fix: more than 32 AlertLogIDs.
* ``CoveragePkg``, ``MemoryPkg``, ``TextUtilPkg``: GHDL compatibility (function purity, ``line`` indexing).


.. _RELEASES/2015:

2015
****

.. _RELEASES/2015.06:

2015.06
=======

:Source: `Git tag 2015.06 <https://github.com/OSVVM/OSVVM/tree/2015.06>`__

Announced in `Announcing OSVVM release 2016.01 <https://osvvm.org/archives/1101>`__ (2016-01-26).

* New ``MemoryPkg`` for sparse memory models.
* ``AlertLogPkg``: ``AffirmIf`` and log level ``PASSED``.
* ``CoveragePkg``: mirroring for ``WriteBin`` and ``WriteCovHoles``.

.. _RELEASES/2015.03:

2015.03
=======

:Source: `Git tag 2015.03 <https://github.com/OSVVM/OSVVM/tree/2015.03>`__

Announced in `Announcing OSVVM 2015.03 <https://osvvm.org/archives/948>`__ (2015-03-29).

* ``AlertLogPkg``: ``AlertIfEqual``, ``AlertIfNotEqual``, ``AlertIfDiff`` (files).
* ``AlertLogPkg``: ``ReadLogEnables``, ``ReportNonZeroAlerts``, ``PathTail``, ``ReportLogEnables``, ``GetAlertLogName``.
* ``AlertIf``/``AlertIfNot`` overloads with ``AlertLogID`` first (old order deprecated); ``FindAlertLogID`` takes a
  ``ParentID``.
* Fixes: ``GetAlertLogID`` no longer shares IDs of equal names; new IDs inherit enables from the parent; ``ClearAlerts``
  restores stop counts.
* 2015.01 withdrawn from downloads due to known bugs.

.. _RELEASES/2015.01:

2015.01
=======

Announced in `Announcing OSVVM 2015.01 <https://osvvm.org/archives/927>`__ (2015-02-05).

* New ``AlertLogPkg``: alerts replace ``assert``; ``FAILURE``/``ERROR``/``WARNING`` counts, enables and stop counts;
  hierarchical AlertLogIDs; end-of-test report; ``Log`` with verbosity control.
* All OSVVM packages use alerts instead of ``assert``.
* New ``TranscriptPkg``: ``TranscriptFile``, ``TranscriptOpen``, ``TranscriptClose``, ``print``, ``writeline``; used by
  ``AlertLogPkg`` and ``CoveragePkg``.
* New ``OsvvmGlobalPkg`` for global coverage and alert settings.
* ``AlertLogPkg`` package body replaceable to map onto other alert packages.


.. _RELEASES/2014:

2014
****

.. _RELEASES/2014.07a:

2014.07a
========

:Source: `Git tag 2014.07a <https://github.com/OSVVM/OSVVM/tree/2014.07a>`__

Announced in `OSVVM 2014.07a: Protected Types, Initialized Pointers, and Memory Leaks
<https://osvvm.org/archives/913>`__ (2014-12-16).

* ``CoveragePkg``: ``deallocate`` frees bin name strings (leak in 2014.07).
* Coverage models in temporary objects (e.g. subprograms) must be deallocated before exit.
* ``NamePkg``: pointer initialization conflicted with protected types; ``Get`` returns ``""`` for a ``NULL`` pointer.

.. _RELEASES/2014.07:

2014.07
=======

Announced in `OSVVM 2014.07 Finally <https://osvvm.org/archives/890>`__ (2014-10-31).

* ``CoveragePkg``: bin names via ``AddBins``/``AddCross`` (for requirements tools); printed by ``WriteBin`` and
  ``WriteCovHoles``.
* ``WriteBin`` prints ``PASSED``/``FAILED`` against ``AtLeast``; parameters select the printed fields.
* ``GenBin(integer_vector)`` creates one bin per value.

.. _RELEASES/2014.01:

2014.01
=======

:Source: `Git tag 2014.01 <https://github.com/OSVVM/OSVVM/tree/2014.01>`__

Announced in `Announcing OSVVM Release 2014.01 <https://osvvm.org/archives/764>`__ (2014-03-20).

* Examples distributed separately from the library.
* New ``MessagePkg``; compile order ``MessagePkg``, ``SortListPkg_int``, ``RandomBasePkg``, ``RandomPkg``,
  ``CoveragePkg``.
* ``CoveragePkg``: merging of coverage databases, tracking of the last randomization, improved overlapping count bins,
  improved reporting on Aldec tools.
* ``RandomPkg``: ``RandTime``, ``RandReal`` for sets; ``RandIntV``, ``RandRealV``, ``RandTimeV``; ``sort``/``revsort``
  aliases.


.. _RELEASES/2013:

2013
****

.. _RELEASES/2013.05:

2013.05
=======

:Source: `Git tag 2013.05 <https://github.com/OSVVM/OSVVM/tree/2013.05>`__

Announced in `Announcing OSVVM 2013.05 <https://osvvm.org/archives/648>`__ (2013-05-29).

* ``CoveragePkg``: ``RandCovPoint`` overload returning ``integer`` (per the author's comment on `RandCovPoint for
  Item/Point coverage <https://osvvm.org/archives/339>`__).

.. _RELEASES/2013.04:

2013.04
=======

Announced in `Announcing OSVVM Release 2013.04 <https://osvvm.org/archives/579>`__ (2013-04-30).


.. _RELEASES/2012:

2012
****

.. _RELEASES/2.3.1:

2.3.1
=====

Announced in `OS-VVM Updates <https://osvvm.org/archives/388>`__ (2012-06-22).

* Minor package source updates after DAC 2012 feedback.
* Simplified ``SENSORS`` example; improved ``FIFO`` testbench syntax.
