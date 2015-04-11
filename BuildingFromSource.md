_Instructions originally contributed by Christian Holm Christensen, thank you!_

You can get the sources for the app at

> hg clone https://code.google.com/p/hackerskeyboard/

(Mercurial repository).

## Developing using Eclipse ##

First off, you need the Android SDK and NDK available at

> http://developer.android.com/sdk
> http://developer.android.com/sdk/ndk

I'll assume you've unpacked these to _~/android/sdk_ and _~/android/ndk_
respectively.  You don't need to do that, but adjust paths accordingly.

Then, you should get the Android Development Tool (ADT) plug-in at

> http://developer.android.com/sdk/eclipse-adt.html

To be effective, you should get the Mercurial plug-in

> http://www.javaforge.com/project/HGE#download

Now, use the _File->New->Project..._ wizard, selecting
_Mercurial->Clone ..._ to check out the sources into your workspace.

Note, that this will check out a directory structure that doesn't really correspond to the directory structure expected by the ADT plug-in. You should therefore use the _File->New->Android Project_ wizard to set up another project.  Make sure you select _Create Project from existing sources_ and select the sub-directory _java_ of the sources you check out before.  The project settings should be filled out automatically - except the project name - it could be something like 'my-hackerskeyboard'

The app contains Native code (C++ code) which must be compiled into a shared library.  This library is then loaded at run-time by the application, and through the Java Native Interface (JNI) protocol (member) functions in that library is called.  However, the standard set-up of the ADT does not compile JNI code on its own.   You should therefore set up a custom builder.

Right click the project and select _Properties_, or select _Project->Properties_ from the menu.  Select _Builders_ and click _New_.   In the dialog, select _Program_.  In the next window, give it a name - say _Android NDK builder_, point the _Location_ to _~/android/ndk/ndk-build_ and set the working directory to the variable _${project\_loc}_.  In the _Refresh_ tab, select _Refresh project_. In the _Build Options_ tag, select _After a "Clean"_, _Manual builds_, _During auto builds_, and _During "Clean"_.

Now, clean the project (perhaps twice) and run the app.

## Other stuff ##

### Make Mercurial ignore some (generated) files ###

Perhaps you want to add the file _.hgignore_ in your check-out directory with content like

```
        syntax: regexp
        ^java/bin$
        ^java/obj$
        ^java/gen$
        ^java/libs$
        ^java/.classpath$
        ^java/.project$
        ^java/default.properties$
        ^.project$
```

so that you do not get 'funny' output from 'hg status' or the like

### Making Changes ###

Creating patches:

> hg diff -w > my\_patch.patch

Applying patches:

> patch . < my\_patch.patch


Happy coding.