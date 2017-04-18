[**HOME**](Home) > [**TECHNICAL DOCUMENTATION**](Technical-Documentation) > [**COMMENTS FOR HTML DOCS**](Html-How-To)

# Commenting Code For HTML
The [TGRSIDetectorHit](https://rawgit.com/wiki/GriffinCollaboration/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/class_t_g_r_s_i_detector_hit.html ) class is a good example of how to comment a class for automatic HTML generation using doxygen. Please see the [doxygen](http://www.doxygen.org/index.html) page for more information about doxygen, and [doxygen & ROOT](https://dberzano.github.io/alice/doxygen/) for tips on using doxygen with ROOT.

### Class Description
[[/images/ClassDesc.png]]

To create the class description, place it in a block of comments such as:
```c++
/////////////////////////////////////////////////////////////////
///
/// \class TGRSIDetectorHit
///
/// This is class that contains the basic info about detector
/// hits:
/// 1. An address.         The whoami for the detector. This is the value used by TChannel::GetChannel(address);
///
/// 2. An "Energy value."  What this is left the parent class, but it is going to return a double.
///
/// 3. A   Time  value.     This should be a time value common for detectors (derived from the timestamp)
///                        Units matter here, I am adobting the ns as the standard.
///
/// 4. A   Position.       Tvector3s are nice, they make doing geometery trival.  Each hit needs one to determine
///                        where it is in space, the actual memory of the thing will be stored here.
///                        ***  This is not actually needed here unless we start do waveform analysis to 
///                        ***  better determine where the hit is.
///
/// 5. The waveform.       Since we are dealing with digital daqs, a waveform is a fairly common thing to have.  It
///                        may not always be present, put it is echoed enough that the storage for it belongs here.
///
/////////////////////////////////////////////////////////////////
``` 
This block of comments should go in the header file, ie the .h file.

### Class Index
[[images/ClassIndexDesc.png]]

To add the class to a specific group (in this case Detectors), add the following three lines at the very top (right after the #ifndef and #define statements):
```c++
/** \addtogroup Detectors
 *  @{
 */
```
and the closing bracket after the class description (just before the #endif):
```c++
/*! @} */
```

### Method Documentation
[[images/FuncDesc.png]]

To create method documentation, place the instructions or details in the first comments after the name of the function using ```///```. Everything until the first line without ```///``` is considered as a valid member function description block.
For example,
```c++
void TGRSIDetectorHit::Clear(Option_t* opt) {
  ///General clear statement for a TGRSIDetectorHit.
  fAddress = 0xffffffff;    // -1
  //fPosition.SetXYZ(0,0,1);  // unit vector along the beam.
  fWaveform.clear();        // reset size to zero.
  fCharge         = 0;
  fKValue         =0;
  fCfd            = -1;
  fTimeStamp      = 0;
  //fDetector       = -1;
  // fSegment        = -1;
  fEnergy         = 0.;
  fBitflags       = 0;
  fPPGStatus      = TPPG::kJunk;
  fCycleTimeStamp = 0;
  fChannel        = nullptr;
}
```
produces the above image.

### Data Member Documentation
[[images/MemberDesc.png]]

To document a data member put a C++ comment behind their declaration in the header file using ```///<```:
```c++
      UInt_t   fAddress;    ///< address of the the channel in the DAQ.
      Float_t  fCharge;     ///< charge collected from the hit
      Short_t  fKValue;      ///< integration value.
      Int_t    fCfd;        ///< CFD time of the Hit
      Long64_t   fTimeStamp;  ///< Timestamp given to hit
      std::vector<Short_t> fWaveform;  ///<
```
For transient members ```//!<!``` has to be used:
```c++
      mutable Double_t fTime;       //!<! Calibrated Time of the hit
```
### Latex
You can also add LaTeX to function and class descriptions inline by putting the LaTeX code between a pair of \f$ commands. Using the \f[ and \f] commands instead creates a new line with the LaTeX code. Formulas or other latex elements that are not in a math environment can be specified using \f{environment}, where environment is the name of the LaTeX environment, the corresponding end command is \f}.

### Authorship
Authorship of a class can be included in the class description at the top of the header file by using the \author command:
```c++
/// \author First Lastname <email@some.thing>, home institute
```
This is worth doing in all files you make to get credit for your hard work!

### HTML generation
HTML documentation will be generated and placed on the wiki periodically. If you would like the documentation to be updated please contact @r3dunlop.
