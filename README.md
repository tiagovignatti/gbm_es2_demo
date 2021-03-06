# GBM ES2 Demo
* Example OpenGL ES2 demo using GBM and DRM(KMS) modesetting
* This shows how ChromeOS renders GLES2 contents on the screen.
* It's the best way for embedded device to use OpenGL ES2/3, because of escape of X11 dependency. (more fast and less battery)

![Alt text](https://github.com/ds-hwang/gbm_es2_demo/blob/master/images/screenshot.jpg "dma_buf_mmap_demo screenshot")

# Build
```
> mkdir build; cp build
> cmake ../
> make
```

# Run
* I successfully run it on Ubuntu, ChromeOS and Yocto.

## Ubuntu
* Go to tty1 with Ctrl + Alt + F1
* Kill gdm or lightdm because they are DRM master now. This demo has to be DRM master.
```
> sudo service lightdm stop
```

* Run the demo
```
> gbm_es2_demo
or
> dma_buf_mmap_demo
```

## Yocto
* The easiest way to build embedded linux image is to use Yocto.
* I make Yocto recipes to make standalone emebeded OpenGL ES2 demo image.
* Check [Yocto GBM ES2 Demo](https://github.com/ds-hwang/yocto-gbm_es2_demo)
* Enjoy building Linux image from the scratch.

# Demo detail
## gbm_es2_demo
* Show how to glue DRM, GBM and EGL
* Show how to swap buffer and vsync
* Show how to implement GLES2 app

## dma_buf_mmap_demo
* Same to gbm_es2_demo except for updating cube surface using dma_buf mmap
* It's very nice demo to show how Chromium zero-copy texture upload works. Chromium doesn't use glTexImage2D to update texture thank to new dma_buf mmap API. For more Chromium detail, check [the chrome issue](crbug.com/475633)
* After Linux kernel v4.6, you can use following code. (currently only Intel Architecture supports it)
```
  void* data = mmap(dma_buf_fd)
  update contents on |data|
  munmap(data)
```

# Code style
* The style complying with [Chromium’s style guide](http://www.chromium.org/developers/coding-style)
* Before submitting a patch, always run `clang-format`
```
> clang-format-3.7 -i *.h *.cpp
```

# Reference
* [overview by jbarnes](http://virtuousgeek.org/blog/index.php/jbarnes/2011/10/31/writing_stanalone_programs_with_egl_and_)
* [My mesa-demo](https://lists.freedesktop.org/archives/mesa-dev/2016-April/114985.html)
* [My ChromeOS dma-test demo](https://chromium-review.googlesource.com/#/c/340953/5)
* [greatest drm tutorial by dvdhrm](https://github.com/dvdhrm/docs)
* [kmscube by robclark](https://github.com/robclark/kmscube)

