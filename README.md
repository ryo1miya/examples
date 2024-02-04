<h1 id="top">my private examples (ABSOLUTELY NO WARRANTY)</h1>

- [glReadPixels and stbi_write_jpg](#write_jpg)
- [glReadPixels and stbi_write_png](#write_png)
- [mpg123 and fzf](#mpg123fzf)

<h2 id="write_jpg">glReadPixels and stbi_write_jpg</h2>

This example depends on [stb_image_write.h](https://github.com/nothings/stb) .

```c
#define STB_IMAGE_WRITE_IMPLEMENTATION
#include "stb_image_write.h"

// include some other headers ...

void saveJPG(char *filename, GLuint w, GLuint h)
{
    int comp = 3; // 1: grayscale, 2: grayscale with alpha, 3: rgb, 4: rgba
    void* buf = NULL;
    buf = ( GLubyte* )malloc( w * h * comp );

    glReadBuffer( GL_BACK ); // or GL_FRONT

    // NOTE: glReadPixels turns an image upside-down
    glReadPixels( 0,                 // x coordinate of the lower left corner of a rectangle
                  0,                 // y coordinate of the lower left corner of a rectangle
                  w,                 // width
                  h,                 // height
                  GL_RGB,            // GL_BGR or GL_RGBA or ...
                  GL_UNSIGNED_BYTE,  // array type
                  buf );             // buffer pointer

    // NOTE: we must flip the image vertically
    void* flip = NULL;
    flip = ( GLubyte* )malloc( w * h * comp );
    int wc = w * comp;
    for (int j=0; j<h; j++) memcpy(flip+(h-1-j)*wc, buf+j*wc, wc);

    /*
       stbi_write_jpg(char const *filename,
                      int x,                // width
                      int y,                // height
                      int comp,             // 1: grayscale, 2: grayscale with alpha, 3: rgb, 4: rgba
                      const void *data,     // buffer pointer
                      int stride_bytes);    // the length of a row (width * comp)
    */
    stbi_write_jpg(filename, w, h, comp, flip, wc);
}
```

[back to top](#top)

<h2 id="write_png">glReadPixels and stbi_write_png</h2>

This example depends on [stb_image_write.h](https://github.com/nothings/stb) .

```c
#define STB_IMAGE_WRITE_IMPLEMENTATION
#include "stb_image_write.h"

// include some other headers ...

void savePNG(char* filename, GLuint w, GLuint h)
{
    int comp = 4; // 1: grayscale, 2: grayscale with alpha, 3: rgb, 4: rgba
    void* buf = NULL;
    buf = ( GLubyte* )malloc( w * h * comp );

    glReadBuffer( GL_BACK ); // or GL_FRONT

    // NOTE: glReadPixels turns an image upside-down
    glReadPixels( 0,                 // x coordinate of the lower left corner of a rectangle
                  0,                 // y coordinate of the lower left corner of a rectangle
                  w,                 // width
                  h,                 // height
                  GL_RGBA,           // GL_BGR or GL_RGBA or ...
                  GL_UNSIGNED_BYTE,  // array type
                  buf );             // buffer pointer

    // NOTE: we must flip the image vertically
    void* flip = NULL;
    flip = ( GLubyte* )malloc( w * h * comp );
    int wc = w * comp;
    for (int j=0; j<h; j++) memcpy(flip+(h-1-j)*wc, buf+j*wc, wc);

    /*
       stbi_write_png(char const *filename,
                      int x,                // width
                      int y,                // height
                      int comp,             // 1: grayscale, 2: grayscale with alpha, 3: rgb, 4: rgba
                      const void *data,     // buffer pointer
                      int stride_bytes);    // the length of a row (width * comp)
    */
    stbi_write_png(filename, w, h, comp, flip, wc);
}
```

[back to top](#top)

<h2 id="mpg123fzf">mpg123 and fzf</h2>

assume m3u playlists are in `~/mp3`

```shell
mpg123 -v@ $(ls -1 ~/mp3/*.m3u | fzf)   # normal play
```

```shell
mpg123 -vz@ $(ls -1 ~/mp3/*.m3u | fzf)  # shuffle play
```
[back to top](#top)
