# Contributing to the code when using cmake

If you want to contribute to the code (fixing bugs you've found, or adding functions/classes you need), and you are using cmake, there are a few extra things to do compared to the old make.
What you need to do depends on whether you are changing existing code, or you are adding new files and/or classes, and whether you are working on GRSISort itself or one of the parser libraries like GRSIData.

## Contributing to GRSISort

### Changing existing code

If you want to change code in GRSISort ifself, the procedure isn't dissimilar to the old one when using make:
 - Edit the files in the source directory (the local clone of your fork of the GRSISort repository).
 - Compile the code in the build directory by changing into that directory and using make or cmake, or by calling cmake with build directory as an argument.
 - Verify that the code works to your satisfaction.
 - Add the changes to git, and push the commits to your fork of GRSISort.

Once you are done, consider making a pull request to make the changes you've made in your private fork available to everyone via the upstream GRIFFINCollaboration repository.

### Adding classes and/or files

If you are adding classes and/or files, you need to let cmake know about these new files.
To do this you need to edit `CMakeLists.txt`.

In most cases you are adding the new class to an existing shared object library, so you'll need to find where that library is defined in `CMakeLists.txt` (search for `add_library(NAME`, where NAME is the name of the library).
Once you've found the library, add the new source file, and if a root dictionary is being created for this class, add the class to the line after the library that starts with `root_generate_library(`.
Of course you still need to edit the LinkDef file accordingly if you add a class with root dictionary, just as with the old make system.

If you are adding a new shared object library, you will need to define it via the `add_library` command, providing the name of the library, and all source files included in it.
You can add a root dictionary using the `root_generate_dictionary` command with a name for the dictionary (should be `G__` and then the name of the library), all header files for classes in the dictionary, `MODULE` (all caps) followed by the name of the library, and `LINKDEF` followed by the path to the LinkDef.h file (starting with `${PROJECT_SOURCE_DIR}` which is the directory where the source is, i.e. the clone of the GRSISort repository).
Finally you can add libraries that this library needs to be linked against via the `target_link_libraries` which takes the name of the new library, followed by all libraries it should be linked against.
To link against ROOT libraries use `${ROOT_LIBRARIES}`.
In rare cases your library might depend on another target i.e. the version header file, this can be included via the `add_dependencies` command which takes the name of the library and the name of the target it depends on (in the case of the version header file this would be `GVersion`).

## Contributing to a parser library like GRSIData

For the parser libraries things are a bit more complicated, as these are normally clones fresh from the upstream repositories into the `_deps/NAME-src/` directory (with `NAME` being the name of the parser libraries in all lower case, e.g. grsidata).
That means anytime cmake is configured (e.g. by calling `cmake -S GRSISort -B GRSISort-build`), any changes made to those directories will be overwritten.

### Changing existing code

If you are only changing existing files, you can work directly in the source code win the `_deps/NAME-src/` directory, you just need to be careful not to configure cmake at any point without first committing and pushing your changes.
You will also need to add your own fork of the parser library as `origin` to git by calling `git remote add origin <fork>`, where `<fork>` is the URL or SSH location of your fork on github.

An alternative to this is to edit the GRSISort `CMakeLists.txt` file and replace the URL in the `FetchContent_Declare` function for the parser library you are working on from the upstream version to your own fork.
Then you can edit your local copy of your fork, push the changes to github, re-configure cmake (e.g. by calling `cmake -S GRSISort -B GRSISort-build`), and re-compile it (e.g. by calling `cmake --build GRSISort-build --target GRSIData`).

### Adding classes and/or files

If you are adding classes and/or files to the parser library, the same approaches described for editing code in the parser library can be used if you also edit the `CMakeLists.txt` file as described in the section for adding classes to GRSISort.
