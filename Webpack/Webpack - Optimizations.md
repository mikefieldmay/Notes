# Optimizations

There are numerous optimisations that we can implement using Webpack.

## Splitting out Vendor code

We can split out our third party vendor code in order to improve performance. This has a positive affect due to how vendor caching works.
When you first visit a website the browser the browser checks to see if it has downloaded the bundle of javascript. If no it downloads the file, but if it has, it will cache that asset (asset caching).
We split the vendor code out because every few months your vendor asset may have updates. Updates to your internal codebase is likely to happen far more frequently. When we split our personal code out, our vendor code does not need to download the vendor assets so frequently. This is very useful for repeat views. You should use `import` when you're splitting your own code. For vendor splitting we should use the webpack config.
The browser will look at the exact name of the file.

The problem we run into is that if we have very specific name e.g `bundle.js` the browser will not re-download the file as it only checks the filename. In order to create a unique identifier we also need to add a chunkhash to the name of the file.
