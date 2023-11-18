# my private examples (ABSOLUTELY NO WARRANTY)

## glReadPixels and stbi_write_jpg

```c
#define STB_IMAGE_WRITE_IMPLEMENTATION
#include "stb_image_write.h"

// include some other headers ...

void saveJPG(char *filename, GLuint w, GLuint h)
{
    int comp = 3; // 1: mono, 2: mono with alpha, 3: rgb, 4: rgba
    void* buf = NULL;
    buf = ( GLubyte* )malloc( w * h * comp );

    glReadBuffer( GL_BACK ); // or GL_FRONT

    // NOTE: glReadPixels turns the image upside-down
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
                      int comp,             // 1: mono, 2: mono with alpha, 3: rgb, 4: rgba
                      const void *data,     // buffer pointer
                      int stride_bytes);    // the length of a row (width * comp)
    */
    stbi_write_jpg(filename, w, h, comp, flip, w * comp);
}
```
