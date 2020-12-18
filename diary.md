# December 2020

Ranting:
---
Maybe I dont know enough, but writing cross-platform application using C/C++ on Windows is super PAINFUL. Its basically Win32 and everything else.

I dont want to explain the pains. There are too many.

Whatever, to ease your life, you can follow Qt and Gtk's practice, that is using MinGW(MSYS2) to build things (Dont have to install Visual Studio at all).

 ___
 
GTK really works hard on breaking API. Look at the migrating guide, you may ask why the hell my app doesn't work anymore? Why do i have to refacter everywhere of my codebase? So my app just got abadoned? Does my app begin to ticking death if it is written in GTK? In a few years, unless the app is trivial or it would break anyway.

<https://developer.gnome.org/gtk4/stable/gtk-migrating-3-to-4.html>
