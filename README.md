Snyder Projection Implementation
================================

Copyright (C) 2012, 2013  Jeremy J. Gibbons

References:

    Snyder, J. P. Map Projections--A Working Manual.
    U. S. Geological Survey Professional Paper 1395.
    Washington, DC: U. S. Government Printing Office, 1987, 1994 3rd Printing.
    http://pubs.er.usgs.gov/publication/pp1395

SnyderVis data provided by:

    A Global Self-consistent, Hierarchical, High-resolution Geography Database
    http://www.soest.hawaii.edu/pwessel/gshhg
    Extracted from version 2.2.3 on 1 July 2013

Features
========
    + Android compatible
    + Thread safe
    + Minimum class topology
    + Small library footprint

Drivers + Deployment
====================

The **lib** + **resources** directories contain the libraries, projection and
ellipsoid configuration parameter files necessary to run only the test drivers.
When creating a distribution jar file (e.g. snyder-*.jar) from the build
system, it is not necessary to include anything from either of the above
directories.

Code Example
============

***Stereographic Projection Example***
```Java
    public static void main(String... args) {
        Projection proj = new Stereographic();
        //
        // WGS 84
        //
        Ellipsoid e = new Ellipsoid(6378137.0, 298.257223563);

        //
        // Print all available parameters
        //
        for(String prop : proj.getDatumProperties())
            System.out.println(prop);

        Datum d = new Datum();

        //
        // Core projection library assumes all long/lat values are in Radians
        //
        d.setUserOverrideProperty("lon0", 0);
        d.setUserOverrideProperty("lat1", 0);
        d.setUserOverrideProperty("k0", 1);

        double[] lon = new double[] {1.0 * SnyderMath.DEG_TO_RAD}; // 1.0 degree
        double[] lat = new double[] {2.0 * SnyderMath.DEG_TO_RAD}; // 2.0 degree

        //
        // [0][] -> x[]
        // [1][] -> y[]
        //
        double[][] forward = proj.forward(lon, lat, e, d);
        System.out.println("x: " + forward[0][0] + ", y: " + forward[1][0]);

        //
        // [0][] -> lon[]
        // [1][] -> lat[]
        //
        double[][] inverse = proj.inverse(forward[0], forward[1], e, d);
        System.out.println(
                "lon: "   + (float) (inverse[0][0] * SnyderMath.RAD_TO_DEG) +
                ", lat: " + (float) (inverse[1][0] * SnyderMath.RAD_TO_DEG));
    }
```

Tidy-up + Features + Tasks
==========================

[ ] Create Proj4 factory class

[ ] Supplant StrictMath.* with SnyderMath.*

[ ] Standardize variable naming (\*, \\, \+, \-)
    (e.g. 1\_MUL\_5, 1\_DIV\_PI, 1\_ADD\_x, 1\_SUB\_x)

[ ] Consolidate and promote common functions to SnyderMath
    (e.g. Conformal latitude function)

[ ] Select new licensing (i.e. permissive)
