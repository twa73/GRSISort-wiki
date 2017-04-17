# **THIS PAGE IS OUT OF DATE!**

[**HOME**](Home) > [**TECHNICAL DOCUMENTATION**](Technical-Documentation) > [**COMMENTS FOR HTML DOCS**](Html-How-To)

# Commenting Code For HTML
The [TFragment](https://rawgit.com/wiki/GriffinCollaboration/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/class_t_fragment.html ) class is a good example of how to comment a class for automatic HTML generation using doxygen. Please see the [doxygen](http://www.doxygen.org/index.html) page for more information about doxygen, and [doxygen & ROOT](https://dberzano.github.io/alice/doxygen/) for tips on using doxygen with ROOT.

### Class Description
[[/images/ClassDesc.png]]
To create the class description, place it in a block of comments such as:
```c++
////////////////////////////////////////////////////////////////
///                                                            
/// \class TFragment                                                  
///                                                            
/// This Class contains all of the information in an event     
/// fragment                                                   
///                                                            
////////////////////////////////////////////////////////////////
``` 
This block of comments should go in the header file, ie the .h file.

### Class Index
[[images/ClassIndexDesc.png]]
To create the class index, place a comment after the ClassDef statement in the header file.
For example,
```c++
ClassDef(TFragment,4);  // Event Fragments
```

### Method Documentation
[[images/FuncDesc.png]]
To create method documentation, place the instructions or details in the first comments after the name of the function. Like in a class description block, everything until the first non-commented line is considered as a valid member function description block.
For example,
```c++
double TChannel::CalibrateTIME(double time)  {
   //Returns the calibrated time. The polynomial time calibration formula is 
   //applied to get the calibrated time. This function does not use the 
   //integration parameter.
  
   if(TIMECoefficients.size()==0)
      return time;

   double cal_time = 0.0;
   for(int i=0;i<TIMECoefficients.size();i++){
      cal_time += TIMECoefficients[i] * pow(time,i);
   }
   return cal_time;
}
```
Produces the above image.

### Data Member Documentation
[[images/MemberDesc.png]]
To document a data member put a C++ comment behind their declaration in the header file.
```c++
   UInt_t PPG;                    //-> Programmable pattern generator value
   UShort_t DeadTime;	          //-> Deadtime from trigger
   UShort_t NumberOfFilters;      //-> Number of filter patterns passed
   UShort_t NumberOfPileups;      //-> Number of piled up hits 1-3
   UShort_t DataType;             //-> 
   UShort_t DetectorType;         //-> Detector Type (PACES,HPGe, etc)
   UInt_t ChannelId;              //-> Threshold crossing counter for a channel

   std::vector<UShort_t>  KValue; //-> KValue for each pileup hit
```
### Latex
You can also add LaTeX to function and class descriptions by surrounding it with "Begin_Latex" and "End_Latex". You can have multiple lines, and e.g. align each line at the '=' sign by passing the argument separator='='.

### Authorship
At the top of a file, you can include authorship by typing
```c++
// Author: name of author 
```
at the very top of the header file. This is worth doing in all files you make to get credit for your hard work!
### HTML generation
HTML documentation will be generated and placed on the wiki periodically. If you would like the documentation to be updated please contact @r3dunlop.