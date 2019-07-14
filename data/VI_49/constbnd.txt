DESCRIPTION OF CONSTELLATION BOUNDARY DATA

   The National Space Science Data Center (NSSDC),  on their "114 Selected
Astronomical Catalogs" CD-ROM,  distributes a file of constellation
boundary data.  The official constellation boundaries were defined in 1930,
dividing the sky into 88 constellations.  The boundaries were specified in
B1875.0 coordinates,  and follow straight lines of right ascension and
declination in that system.  They therefore resemble the state borders of
some of the Western United States,  which follow lines of latitude and
longitude.

   These data can now be found on VizieR at

http://cdsweb.u-strasbg.fr/viz-bin/ftp-index?VI/49

   Included are a data file in the original B1875.0 system,  and one
in the J2000.0 system.  In the second file, the lines no longer
coincide with meridians and parallels,  and "shape points" are added
to define what has become a curve.  For each constellation,  a list
of its border coordinates is given in each file.

   For most of my purposes,  the 1875.0 file was the important one.  If
you want to know in which constellation a particular point lies,  it is
easiest to first convert it to an 1875.0 position;  you can then check it
against the (perfectly "vertical" and "horizontal") 1875.0 constellation
borders.  If you use J2000.0 coordinates instead,  the borders run at
odd angles,  and your work is increased.  (Details on how to determine
in which constellation a particular point lies are given below.)

   I first wrote software to find out which constellation lay on
_either_ side of a given border.  This had three uses.  First,  if I
wanted to label both sides of a border,  I could easily do so  ("OK,
label this segment SGR on the right side,  OPH on the left side").
Second,  I could avoid drawing the same segment twice ("Aha!  This
segment is OPH on the right and SGR on the left... I already drew it,
running the other way!").  Since the original file contains every
segment twice (once as a part of the border of the constellation on the
left side,  and again as part of the border of the constellation on the
right side),  this was no small concern.

   Third, it allowed me to check for mismatches ("The border as
defined for SER doesn't match the one defined for OPH").  In fact,
this mismatch did occur:  the border for Serpens didn't match up the
border for Ophiuchus. This (small) error is corrected in GUIDE's
boundary data (as discussed below).

   Each line gives an 1875.0 RA and declination (in decimal hours and
degrees),  the constellation to the left of the line segment ("inside"),
and the one to the right ("outside").  For an example,  consider the
simplest case,  Crux:

11.83333 -55.00000 Cru
12.83333 -55.00000 Cru  Cen
12.83333 -64.00000 Cru  Cen
11.83333 -64.00000 Cru  Mus
11.83333 -55.00000 Cru  Cen

   This constellation is a simple rectangle in 1875.0 coordinates.  It
starts at 11h50m, S 55;  runs east-west to 12h50m,  with Centaurus above
it;  then extends south nine degrees to declination S 64,  with Centaurus
across the border;  then runs back to 11h50m,  with Musca the Fly to the
south;  then completes the circuit by returning to 11h50m, S 55,  with
Centaurus again across the border.

   You'll notice that the first point is identical to the last,  and that
the first point in a constellation border has no constellation "across"
from it;  i.e.,  there are five lines listed for Crux,  but only the last
four have "opposite" constellations.  For most purposes,  you can ignore
the "opposite" constellations.  But as I have said,  that data sometimes
is of use.

   Two final hints for using this data:   Remember that there is a
discontinuity of sorts between 24h and 0h,  and remember that Ursa Minor
and Octans are special cases:  they enclose celestial poles.  It's not
hard to work around these,  but you do need to keep them in mind.

OTHER DIFFERENCES BETWEEN THIS DATA AND THE ORIGINAL DATA:

   Because I added a "closing point" (for example,  the rectangular
constellation of Crux has five points in my data instead of the four
given in the original),  I added 89 points to the data (there are 88
constellations,  and Serpens makes up two polygons).  I removed four
unnecessary points from the original data:

 1.00000 +88.00000 CEP  O
 0.00000 -90.00000 OCT  O
24.00000 -90.00000 OCT  O
12.00000 -90.00000 OCT  O

   (The Octans points were presumably added because of some problem
encountered with the south celestial pole discontinuity.  I don't know
why the extra point was added in Cepheus;  it serves no useful purpose.)
After adding 89 points and removing four,  I ended up with 85 more points
than were in the original dataset.

   Also,  I lowercased constellation abbreviations (CMA became CMa, etc),
and added leading zeroes for decs -10 to +10.  I reversed the order of
about half the constellations so that they'd all be counterclockwise and
added "opposite side" constellation IDs.

   Also,  there was a small inconsistency along the Ophiuchus-Serpens
(Cauda, the western half) border.  In the original data,  for the
part of the border between decs +3 to +4.5,  the RA is given as 18h
25.5m (B1875) on the Serpens side,  but as 18h 25.3m on the Ophiuchus
side. This was the only mismatch found.  For the nonce,  I'm using
the 25.5m value.

THE "WHAT CONSTELLATION IS THIS POINT IN" PROBLEM:

   This question comes up frequently.  If you've got data such as that
described above,  it can be reduced to the more generic problem of
"is this point inside or outside a given polygon",  because the
constellations have been described as 89 distinct polygons.  You
just run through each polygon until the answer is "yes,  this point
is inside the polygon."  Thus,  the problem is a "geometry" one,
not "astronomy".

   You may need to switch to a fixed-size font to make heads or
tails of the following diagram.  It shows a polygon with sides
that are vertical or horizontal,  much like the constellation
data is when we're working in B1875 coordinates.

   ,---------------------,
   |@                    |@         B
   |                     '-----,
   |                           |
   |        ,-----,            |
   |*       |*    |*    A      |
   |        |     |            |
   |        |     '------------'    C
   `--------'

   The "point in polygon" problem is solved by looking at an
imaginary ray running from the point to the left.  We ask ourselves:
where does that ray intersect the polygon?  For the point B,  there
are two intersection points (labelled with @s).  For the point A,
there are three (labelled with *s).

   Looking at it from the other direction,  with the ray running
from left to right,  the ray starts out "outside" the polygon.
Then it goes "inside",  then "outside",  then "inside",  and
would go "outside" again.  So:

   If the point is inside the polygon,  the ray will intersect
that polygon an odd number of times.  If it's outside the
polygon,  the ray will intersect it an even number of times.

   Suppose the point is at (x, y),  and the polygon is described
by a series of segments.  We'll call each segment (x1, y1) to
(x2, y2).  Our pseudocode looks like this:

n_intersections = 0;
for each segment in the polygon:
   if( y1 < y and y2 >=y,  or y2 < y and y1 >=y)
      if( x1 < x)          /* we've got another intersection */
         n_intersections++;
if( n_intersections is even)
   return( FALSE);               /* the point isn't in the polygon */
else
   return( TRUE);               /* the point is in the polygon */

   The use of '>' vs '>=' and such is very important.  Look at
point C,  and you can perhaps see why:  there are a couple of
segments where the y-values of the end points are equal to that
of point C,  and we want to make sure that we either consider
both to be "intersections" or neither to be "intersections".
(The compound 'if' statement there could be translated to be
"if the line runs _up_ past the point,  or if it runs _down_
past the point",  with "x1 < x" being "the segment is to the
left of the test point.")

   Two "gotchas" mentioned above still apply:  unlike in normal
plane geometry,  constellations sometimes cross from 0 to 24
hours,  and Octans and Ursa Minor require some extra care.
