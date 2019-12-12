# memCompression

This is a started from gzmem and Rcompression.  It was made 
into a new repo because we are hoping to add some different 
functionality.

### Usage

This package contains functions to compress and decompress raw vectors in R using the [zlib library](https://zlib.net/).

It supports 3 different formats: gzip, zlib and raw. These formats all use the [deflate algorithm](https://en.wikipedia.org/wiki/DEFLATE) to compress the data, but differ in the headers and footers they wrap around the deflate compressed data:

-   gzip has a 10 byte header and an 8 byte footer around the compressed data.
-   zlib has a 2 byte header and a 4 byte footer around the compressed data.
-   raw has no header or footer, and so is just the deflate compressed data.

For example:

``` r
text <- "Hey, the room is getting smaller. No, it's not. *He's* getting *bigger*!"
```

Shows the following differences between all 3 formats:

``` r
gzip <- text %>% 
  charToRaw() %>% 
  memCompression::compress(format = "gzip")
```

    ##  [1] 1f 8b 08 00 00 00 00 00 04 03 3d ca b1 09 c0 30 0c 04 c0 55 3e 95 41 18 cf e1 2a 3b 24 f0 28 02 db 02 4b 4d b6 4f
    ## [39] 97 ab af f3 ad c8 87 d8 ee 13 16 50 66 da 52 c4 bc c6 e0 6e 38 bd c2 b2 04 96 67 83 74 96 90 7f c9 6d aa dc 72 7c
    ## [77] ac 89 c5 99 48 00 00 00

``` r
zlib <- text %>% 
  charToRaw() %>% 
  memCompression::compress(format = "zlib")
```

    ##  [1] 78 01 3d ca b1 09 c0 30 0c 04 c0 55 3e 95 41 18 cf e1 2a 3b 24 f0 28 02 db 02 4b 4d b6 4f 97 ab af f3 ad c8 87 d8
    ## [39] ee 13 16 50 66 da 52 c4 bc c6 e0 6e 38 bd c2 b2 04 96 67 83 74 96 90 7f c9 6d aa dc 72 7c 7c b9 17 ee

``` r
raw <- text %>% 
  charToRaw() %>% 
  memCompression::compress(format = "raw")
```

    ##  [1] 3d ca b1 09 c0 30 0c 04 c0 55 3e 95 41 18 cf e1 2a 3b 24 f0 28 02 db 02 4b 4d b6 4f 97 ab af f3 ad c8 87 d8 ee 13
    ## [39] 16 50 66 da 52 c4 bc c6 e0 6e 38 bd c2 b2 04 96 67 83 74 96 90 7f c9 6d aa dc 72 7c

These can then be decompressed by selecting the appropriate format:

``` r
gzip %>%
  memCompression::decompress(format = "gzip", 1024) %>% 
  rawToChar()
```

    ## [1] "Hey, the room is getting smaller. No, it's not. *He's* getting *bigger*!"

