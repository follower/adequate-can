# List of CAN/CANopen frameworks/libraries & debug/analysis related tools 

> _Context:  
> In early 2019 I evaluated a number of CAN/CANopen tools/libraries for a client project and wrote up some observations. I subsequently got the OK from the client to share my notes publicly, so in case they're useful to you, here they are..._

### GUI-based

 * [SavvyCAN](http://savvycan.com/) -- "CAN bus reverse engineering and capture tool" (via SocketCAN). Includes visualisations & scripting.

 * [CANdevStudio](https://github.com/GENIVI/CANdevStudio) -- relatively early stage "development tool for CAN bus simulation" via the "Open Source InVehicle Infotainment Alliance".

 * [Kayak](https://dschanoeh.github.io/Kayak/) -- "application for CAN bus diagnosis and monitoring." (Seems to have potential but requires [socketcand](https://github.com/dschanoeh/socketcand) rather than working with SocketCAN directly.)

 *  [Qt CAN 2.0](http://gitlab.unique-conception.org/qt-can-2.0) includes:
    * [Virtual test-bench tooling](http://gitlab.unique-conception.org/qt-can-2.0/qt-can-tool-cantestbench)
    * [CANopen protocol](http://gitlab.unique-conception.org/qt-can-2.0/qt-can-protocol-canopen)
    * See also: [here](http://gitlab.unique-conception.org/explore/projects?name=CAN&sort=latest_activity_desc) & [here](http://unique-conception.org/portfolio?fullstory=qt-can-v2&lang=en)

 * [Wireshark CANopen dissector](https://wiki.wireshark.org/CANopen)
    * Not entirely obvious how to select CANopen but use "Decode As..." then change "Current" value of "CAN next level dissector" to CANOPEN. (It seems like it used to be changed in a different place/way, see "[Enable CANopen Protocol](https://osqa-ask.wireshark.org/questions/41447/enable-canopen-protocol)" & "[Capturing and Analyzing CAN Frames with Wireshark](https://libbits.wordpress.com/2012/05/07/capturing-and-analyzing-can-frames-with-wireshark/)".) 
    * See also: [`packet-canopen.c`](https://github.com/wireshark/wireshark/blob/master/epan/dissectors/packet-canopen.c) / "[Bug 6651 - Add dissector for CANopen protocol](https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=6651)"
    * Unless more sophisticated analysis/filtering is required it doesn't seem using Wireshark provides immediate advantage over other CAN/CANopen-specific tools.

 * [CANopen Object Browser / CAN Monitor using SocketCAN](http://www.uv-software.de/dokuwiki/doku.php?id=uvs:programs)

 * [CANToolz](https://github.com/CANToolz/CANToolz) -- "Black-box CAN network analysis framework"

 * [openCanSuite](https://github.com/sebi2k1/openCanSuite) -- "set of tools for analyzing, simulating and visualizing a CAN system" (seems outdated/unmaintained)


### CLI

 * [canutils (PTX flavour)](https://git.pengutronix.de/cgit/tools/canutils/tree/)
    * Includes `candump` with e.g. [different low level config options](https://github.com/jmore-reachtech/can-utils/blob/master/src/candump.c#L37-L49) ([via](https://www.toradex.com/community/questions/3190/apalis-imx6-can-lib-socket-api-issue-with-ethernet.html?childToView=3197#comment-3197))

 *  [openCANopen](https://github.com/marel-keytech/openCANopen) -- Not well documented.

 * See also: `libcanopen` & `eerimoq/cantools`


### Libraries/Frameworks

 * [`libcanopen`](https://github.com/rscada/libcanopen/) -- C library + tools (nmt control, pdo/sdo up/down, monitor, ds401) + Python bindings.
    * Functional but had to work around a couple of issues (now [documented on the project's issue tracker](https://github.com/rscada/libcanopen/issues/created_by/follower)). Has (apparent) advantage over CANopenSocket that it doesn't require a master daemon running.
    * See also: "[Moving motors from a Python script](https://basdebruijn.com/2015/07/moving-motors-from-a-python-script/)"

 * Qt 5 CAN Bus / Qt Serial Bus support
    * [CAN Bus example](https://doc.qt.io/qt-5/qtserialbus-can-example.html)

 * [`can4python`](https://github.com/caran/can4python) -- for Python 3 and reads configuration via `kcd` format.

 * [`eerimoq/cantools`](https://github.com/eerimoq/cantools) -- "CAN BUS tools in Python 3" (Also generates C helpers.)

 * [comFramework](https://sourceforge.net/projects/comframe/) -- "Framework for CAN communication interfaces including code generator"

 * [`ros_canopen`](https://github.com/ros-industrial/ros_canopen) -- "CANopen driver framework for ROS" (See: <http://wiki.ros.org/ros_canopen>)

 * [CANopen for Python](https://github.com/christiansandberg/canopen) -- "supports the most common parts of the CiA 301 standard in a *Pythonic* interface."


### Network file formats & conversion

 * [kcd](https://github.com/julietkilo/kcd) -- "open format to describe communication relationships in Controller Area Networks (CAN)." (Used by Kayak.)

 * [`canmatrix`](https://github.com/ebroecker/canmatrix) -- "Convert CAN (Controller Area Network) Database Formats e.g. .arxml .dbc .dbf .kcd"

 * [Coder DBC](http://www.coderdbc.com/) -- "generate C-code for your CAN Matrix described by .DBC file"

 * [`dbcc`](https://github.com/howerj/dbcc) -- "CAN DBC to C (and XML) compiler"

 * [CANBabel](https://github.com/julietkilo/CANBabel) -- "conversion tool for CAN database files"



## CANopenPIC notes

```
// Power on
  can0  RX - -  730   [1]  00     // NMT Heartbeat NodeID = 0x30 (State = Bootup)
  can0  RX - -  1B0   [2]  00 00  // TxPDO1, DI, NodeID = 0x30 aka 48 -- current button state
  can0  RX - -  730   [1]  05     // NMT Heartbeat NodeID = 0x30 (State = Operational)
  can0  RX - -  730   [1]  05     // NMT Heartbeat NodeID = 0x30 (State = Operational)
```

See:

 * [CAN Identifier List](https://infosys.beckhoff.com/english.php?content=../content/1033/bk51x0/3130089099.html) -- default identifier allocation
 * [CANopen Basics - Guarding and Heartbeat](https://www.canopensolutions.com/english/about_canopen/guarding_heartbeat.shtml)
 * [Network management (NMT)](https://canopen.readthedocs.io/en/latest/nmt.html)  — canopen documentation
 * [Default CAN-ID/COB-IDs summary](https://sourceforge.net/p/canopennode/discussion/387151/thread/fd1d4a09/#eb46) - CANopenNode mailing list post
