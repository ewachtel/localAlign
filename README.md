The align executable described below is available in the build directory.  The code will be made available.  I am modifying some structures for speed and simplicity and cleaning up a bit.  The algorithm will remain the same as described below.  

Regards,

Ed

NAME
align

BUILD
Within build directory:
align #Ubuntu v14.04 (Intel(R) Xeon(R) CPU)
align_mac #mac OS v10.12 (Intel(R) i5-5250U CPU)

SYNOPSIS
align [baseFile] [searchFile] minHitSz minHitMerge minFinalHitSz maxWt cacheLen maxWtMerge startCarry [maxFinalHitWt]

DESCRIPTION
The below two phases 1,2 and 3,4 describe the arguments:
  1) search for key matches of maxWt misses per minHitSz up to cacheLen, cacheLen=k*maxWt
  2) save each cacheLen found
  3) For each cacheLen, search forward and backward for maxWtMerge misses per minHitMerge size.
  4) Save as a match if forward or backward is > minFinalHitSz-cacheLen

baseFile: first string to search (default base.txt)
searchFile: second string to search (default search.txt)
maxWt: phase 1 max char mismatches for minHitSz chars e.g. maxWt=2 abcde matches axcxde
minHitSz: phase 1 size to search with up to maxWt char misses
maxWtMerge: phase 2 max char mismatches for minHitMerge chars e.g. maxWtMerge=2 abcde matches axcxde
minHitMerge: phase 2 size to search with up to maxWtMerge char misses
cacheLen: phase 1 minimum size of a match to keep as a key. cacheLen = k*minHitSz
minFinalHitSz: phase 2 a match must be >= minFinalHitSz-cacheLen
maxFinalHitWt: Max wt a match can have, even with carry (default INT_MAX)
     A match will carry unused density as it searches.  For example, for a phase 2, 1 miss per size 3 segment,
     if the first 3 chars match, there is a 1 carry.  If the second 3 chars have 2 misses, the carry
     is used and the search proceeds with 0 carry.  If, say, 2 genes match to 90% and phase 2 is set for 1
     miss per size 3, the carry will continue to grow for a match. maxFinalHitWt gives the maximum final weight a 
     minFinalHitSz match can have.  This avoids flowing too far past the end of a match 
startCarry: Extra misses allowed for first minFinalHitSz following the cacheLen key.

EXAMPLES
align [baseFile] [searchFile] minHitSz minHitMerge minFinalHitSz maxWt cacheLen maxWtMerge startCarry [maxFinalHitWt]

align bs.txt srch.txt 6 3 20 1 6 1 0 
align 3 3 10 1 6 1 0 
align 4 3 10 0 4 1 0 
align largeBs.txt largeSrch.txt 10 3 40 0 10 1 0
