---
layout: post
title: "Multisampling Anti-aliasing Offscreen Rendering"
categories: coding
tags: [opengl, rendering]
use_math: true
---

## Frame Buffer Object without Multisampling
0. Assume all the data we have defined in an array:
    ```cpp
    enum FBORenderTarget
    {
        NORMAL_FBO,
        NORMAL_TEXTURE,
        NORMAL_COLOR_RBO,
        NORMAL_DEPTH_RBO,
        MULTISAMPLING_FBO,
        MULTISAMPLING_TEXTURE,
        MULTISAMPLING_COLOR_RBO,
        MULTISAMPLING_DEPTH_RBO,
    };
    GLuint RenderRelatedIds[8];
    int rw, rh; // <== This is the specified render canvas size, may not be the same as window size.
    ```

1. Create a normal texture
    ```cpp
    glGenTextures(1, &RenderRelatedIds[NORMAL_TEXTURE]);
    glBindTexture(GL_TEXTURE_2D, RenderRelatedIds[NORMAL_TEXTURE]);
    {
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);
        glTexParameterf(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);
        glTexParameteri(GL_TEXTURE_2D, GL_GENERATE_MIPMAP, GL_TRUE);
    }
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGBA8, rw, rh, 0, GL_RGBA, GL_UNSIGNED_BYTE, 0);
```

2. Create a color render buffer object and a depth render buffer object
    ```cpp
    glGenRenderbuffers(1, &RenderRelatedIds[NORMAL_COLOR_RBO]);
    glBindRenderbuffer(GL_RENDERBUFFER, RenderRelatedIds[NORMAL_COLOR_RBO]);
    glRenderbufferStorage(GL_RENDERBUFFER, GL_RGBA8, rw, rh);
    glGenRenderbuffers(1, &RenderRelatedIds[NORMAL_DEPTH_RBO]);
    glBindRenderbuffer(GL_RENDERBUFFER, RenderRelatedIds[NORMAL_DEPTH_RBO]);
    glRenderbufferStorage(GL_RENDERBUFFER, GL_DEPTH_COMPONENT, rw, rh);
    ```

3. Create and bind the Frame Buffer Object (FBO)
    ```cpp
    glGenFramebuffers(1, &RenderRelatedIds[NORMAL_FBO]);
    glBindFramebuffer(GL_FRAMEBUFFER, RenderRelatedIds[NORMAL_FBO]);
    ```

4. Attach the texture, color render buffer object and the depth render buffer object to the FBO
    ```cpp
    glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_TEXTURE_2D, RenderRelatedIds[NORMAL_TEXTURE], 0);
    glFramebufferRenderbuffer(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_RENDERBUFFER, RenderRelatedIds[NORMAL_COLOR_RBO]);
    glFramebufferRenderbuffer(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT, GL_RENDERBUFFER, RenderRelatedIds[NORMAL_DEPTH_RBO]);
    ```

5. Render the scene
    ```cpp
    GLenum status = glCheckFramebufferStatus(GL_FRAMEBUFFER); // Check the FBO is ready. 
    glViewport(0, 0, rw, rh);
    draw();
    updateGL();
    ```

6. Read the pixels
    ```cpp
    cv::Mat img(rh, rw, CV_8UC4, cv::Scalar(0, 0, 255, 255)); // For example we use a OpenCV Mat to store the pixels.
    glReadPixels(0, 0, rw, rh, GL_RGBA, GL_UNSIGNED_BYTE, img.data);
    cv::cvtColor(img, img, CV_BGRA2RGB);
    cv::flip(img, img, 0);
    ```

7. Switch the FBO back to screen
    ```cpp
    glBindFramebuffer(GL_FRAMEBUFFER, 0);
    glViewport(0, 0, width(), height()); // width(), height() are functions returning the actual window size.
    ```

8. Delete the created texture, color render buffer object, depth render buffer object and the FBO
    ```cpp
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_FBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_COLOR_RBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_DEPTH_RBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_TEXTURE]);
    ```

After the aforementioned steps, the pixels will be stored in `cv::Mat img`, and you can further process it.

### Frame Buffer Object with Multisampling
If we combine multisampling with FBO to implement an off-screen rendering, the previous steps are also necessary. Because a multi-sampling FBO cannot be directly read pixels from. We must have a normal FBO and "blit" the multi-sampling FBO to it. A normal FBO can be directly read pixels from. See http://stackoverflow.com/questions/765434/glreadpixels-from-fbo-fails-with-multisampling for reasons.

The code is like this:

0. Create a multisampling texture.
    ```cpp
    glGenTextures(1, &RenderRelatedIds[MULTISAMPLING_TEXTURE]);
    glBindTexture(GL_TEXTURE_2D_MULTISAMPLE, RenderRelatedIds[MULTISAMPLING_TEXTURE]);
    {
        glTexParameterf(GL_TEXTURE_2D_MULTISAMPLE, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
        glTexParameterf(GL_TEXTURE_2D_MULTISAMPLE, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
        glTexParameterf(GL_TEXTURE_2D_MULTISAMPLE, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);
        glTexParameterf(GL_TEXTURE_2D_MULTISAMPLE, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);
        glTexParameteri(GL_TEXTURE_2D_MULTISAMPLE, GL_GENERATE_MIPMAP, GL_TRUE);
    }
    glTexImage2DMultisample(GL_TEXTURE_2D_MULTISAMPLE, 4, GL_RGBA, rw, rh, GL_TRUE);
    ```

1. Create a multisampling color render buffer object and a multisampling depth render buffer object.
    ```cpp
    glGenRenderbuffers(1, &RenderRelatedIds[MULTISAMPLING_COLOR_RBO]);
    glBindRenderbuffer(GL_RENDERBUFFER, RenderRelatedIds[MULTISAMPLING_COLOR_RBO]);
    glRenderbufferStorageMultisample(GL_RENDERBUFFER, 4, GL_RGBA8, rw, rh);
    glGenRenderbuffers(1, &RenderRelatedIds[MULTISAMPLING_DEPTH_RBO]);
    glBindRenderbuffer(GL_RENDERBUFFER, RenderRelatedIds[MULTISAMPLING_DEPTH_RBO]);
    glRenderbufferStorageMultisample(GL_RENDERBUFFER, 4, GL_DEPTH_COMPONENT, rw, rh);
    ```

2. Create and bind the multisampling frame buffer
    ```cpp
    glGenFramebuffers(1, &RenderRelatedIds[MULTISAMPLING_FBO]);
    glBindFramebuffer(GL_FRAMEBUFFER, RenderRelatedIds[MULTISAMPLING_FBO]);
    ```

3. Attach the multisampling texture, color render buffer, depth render buffer to FBO.
    ```cpp
    glFramebufferTexture2D(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_TEXTURE_2D_MULTISAMPLE, RenderRelatedIds[MULTISAMPLING_TEXTURE], 0);
    glFramebufferRenderbuffer(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, GL_RENDERBUFFER, RenderRelatedIds[MULTISAMPLING_COLOR_RBO]);
    glFramebufferRenderbuffer(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT, GL_RENDERBUFFER, RenderRelatedIds[MULTISAMPLING_DEPTH_RBO]);
    ```

4. Render the scene.
    ```cpp
    GLenum status = glCheckFramebufferStatus(GL_FRAMEBUFFER); // Check the FBO is ready. 
    glViewport(0, 0, rw, rh);
    draw();
    updateGL();
    ```

5. Blit the multisampling FBO to the normal FBO
    ```cpp
    glBindFramebuffer(GL_READ_FRAMEBUFFER, RenderRelatedIds[MULTISAMPLING_FBO]);
    glBindFramebuffer(GL_DRAW_FRAMEBUFFER, RenderRelatedIds[NORMAL_FBO]);
    glDrawBuffer(GL_BACK);
    glBlitFramebuffer(0, 0, rw, rh, 0, 0, rw, rh, GL_COLOR_BUFFER_BIT, GL_NEAREST);
    glBindFramebuffer(GL_FRAMEBUFFER, RenderRelatedIds[NORMAL_FBO]);
    ```

6. Read the pixels
    ```cpp
    cv::Mat img(rh, rw, CV_8UC4, cv::Scalar(0, 0, 255, 255)); // For example we use a OpenCV Mat to store the pixels.
    glReadPixels(0, 0, rw, rh, GL_RGBA, GL_UNSIGNED_BYTE, img.data);
    cv::cvtColor(img, img, CV_BGRA2RGB);
    cv::flip(img, img, 0);
    ```

7. Switch the FBO back to screen
    ```cpp
    glBindFramebuffer(GL_FRAMEBUFFER, 0);
    glViewport(0, 0, width(), height()); // width(), height() are functions returning the actual window size.
    ```

8. Delete the created normal and multisampling texture, color render buffer object, depth render buffer object and the FBO
    ```cpp
    glDeleteFramebuffers(1, &RenderRelatedIds[MULTISAMPLING_FBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[MULTISAMPLING_COLOR_RBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[MULTISAMPLING_DEPTH_RBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[MULTISAMPLING_TEXTURE]);
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_FBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_COLOR_RBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_DEPTH_RBO]);
    glDeleteFramebuffers(1, &RenderRelatedIds[NORMAL_TEXTURE]);
    ```

After all these step down, you can get a multisampling rendered scene.
