## BGFX
* When using bgfx::createIndexBuffer, list of indices to pass to bgfx::makeRef should be uint16 in order for it to copy the data properly
* Looks like you should be using CULL_CCW to make sure that backface culling works well (at least in the test program it was the case)