This repo does not contain any code. It is a repo to discuss `lidR v2.0`, a major re-engineering of the `lidR` package.

# Context

The `lidR` package was initially built upon "personal R scripts" that I wrote during my PhD. These scripts were written for my own use at a time when I was less skilled both in R and C++, and at the time I had no plans to build a big framework. I then cleaned up my code and gathered it together in an R package. Before there was any interest from the LiDAR community I released `lidR` on CRAN and I am now continuing its development by adding new algorithms, new functions and new capabilities mainly for the community (I personally don't use `lidR` that much). `lidR` is now a framework with something like 10,000 lines of codes both in R or C++ with many functions and tools.

# Problem

The package is now a relatively big framework (at least for me as a single developer) built upon a perfectly unstructured base. It started simply as a collection of R scripts for personal use, and any additional functionality was appended in a very unorganised manner. For example:

* I made the `rlas` package after the `lidR` package and I redesigned `lidR` several times because of the `rlas` improvements, fighting against my own changes to preserve retro-compatibility of `lidR` versions.
* I added the `LAScatalog` capabilities, initially in a very naive and inefficient way, then I re-wrote everything to build a better frame around `LAScatalog`, thus breaking the retro-compatibility of `lidR`. Right now I still need to break everything because how `lidR`is built does not enable me to make it exactly the way I want it.
* Functions are not all built in a consitent way and that prevents the introduction of new methods without breaking the retro-compatibility.
* And so on... I was stuck many many times because when I made a feature, I didn't plan that it will evolve and I certainly never planned to have so many users.

So in short, `lidR` is like a large building that is built on top of a very tiny house! If I want to add new tools and features it is almost certainly going to collapse. I cannot improve the package further without building a new strong base and that implies breaking many things. And this is exactly what I will do. I will break things!

I now havea good overview of what `lidR` is and what I want it to be. I have chosen to break the compatibility with versions `1.x.y` by writing a version `2` that will come with a much more robust framework.

# What is the goal of this repo?

I created this repo to discuss with users about what `lidR v2.0` should be. Github issues will be used like a forum.

1. I know what I want to do but I would like to know what you would like to see in the future of `lidR`. Here I'm talking about general capabilities rather than specific algorithms (see also next section)
2. I know how I want to redesign some stuff but I will explain my plan first in an issue and I hope to get users' feedback and advice. This way I will not be stuck anymore with features I can't make because of a weak design that does not consider future potential use.
3. Write down my ideas somewhere in a more organized way.

# What will be the main changes in `lidR v2.0`?

* All the functions will be compatible with `LAScatalog`. Currently only `grid_*` functions can process seamlessly
with a `LAScatalog`. Now, `lastrees`, `lasfilterdecimate`, `lasnormalize`, `tree_detection` and so on will also be compatible. This requires a complete re-engineering of the `LAScatalog` internal drivers.
* The package will be fully compatible with `raster`, `sp` and `sf`. This means two things:
    * functions will return or accept `RasterLayers`, `RasterStack`, `SpatialPointDataFrame` and other `SpatialThings` or `SimpleFeature` in a consistent way. No more `data.table` with a class `lasmetrics`, better support of the CRS and so on...
    * main functions from `raster`, `sp` or `sf` such as `extent`, `buffer`, `crop` will be made available on `lidR` objects with the same meaning as in the original package (for example currently `buffer` is overwritten by `raster` because they do not have the same meaning).
* All the functions will be extensible to new algorithm. Two examples:
    * There are 5 `lastrees` functions: the wrapper `lastrees` and the different algorithms `lastrees_li`, `lastrees_dalponte`, etc. `lastrees` is therefore fully extensible to new algorithms.
    * There is only one `tree_detection` function that implements a local maximum filter. I currently can't add new algorithms with breaking retro-compatibility.
    
Also algorithms will be improved so they are faster, and new algorithms and functions will be made available (but this is another question...)
    

lidR v2.0, a major re-engineering of the lidR package
