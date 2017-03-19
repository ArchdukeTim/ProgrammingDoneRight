Morphological Operations
========================

Binary images may contain noise (pixels that passed the initial threshold test but are not desired). Morphological image processing attempts to remove the noise of images while accounting for the form and structure of the image, as well as preserving the desired pixels' integrity. This guide dives into the math behind morphological operations, and then explains how they can be used in FRC.

.. figure:: ../vision/media/example.png
    :width: 120px
    :align: center
    :height: 60px
    :alt: A Binay Image
    :figclass: align-center

Morphological image processing is a collection of non-linear operations related to the shape or morphology of features in an image. Morphological operations rely only on the relative ordering of pixel values and not on their numerical values, therefore making them especially suited to process binary images.

The core idea in binary morphology is to probe an image with a simple, pre-defined shape, and then draw conclusions on how this shape fits or misses the shapes in the image. This simple "probe" is called kernel, and is itself a binary image (i.e., a subset of the space or grid), The kernel dimensions are not limited to 3x3, but OpenCV limits them to be :math:`N\times N` where :math:`N \in \mathbb{Z}_{odd}`, so here we will make the same restrictions.

Here are two common kernels used:

:math:`\left( \begin{array}{ccc} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{array} \right)`    :math:`\left( \begin{array}{ccc} 0 & 1 & 0 \\ 1 & 1 & 1 \\ 0 & 1 & 0 \end{array} \right)`


.. tabs::

   .. code-tab:: java

       Mat kernel = Imgproc.getStructuringElement(Imgproc.RECT, new Size(3,3));
       Mat kernel = Imgproc.getStructuringElement(Imgproc.MORPH_CROSS, new Size(3,3));

   .. code-tab:: c++

       Mat kernel = cv::getStructuringElement(MORPH_RECT, Size(3,3) Point(-1,-1);
       Mat kernel = cv::getStructuringElement(MORPH_CROSS, Size(3,3) Point(-1,-1);

   .. code-tab:: py

       kernel = cv2.getStructuringElement(cv2.MORPH_RECT,(3,3))
       kernel = cv2.getStructuringElement(cv2.MORPH_CROSS,(3,3))


Defintion: Let :math:`E` be a Euclidean space :math:`\mathbb{R}^d` or an integer grid :math:`\mathbb{Z}^d` for some dimension :math:`d`, and :math:`A` be a a binary image where :math:`A \in E`.

Erosion
-----------------

The erosion of the binary image :math:`A` by the kernel :math:`B` is defined by:

:math:`A\ominus B=\bigcap _{{b\in B}}A_{{-b}}.`

.. figure:: ../vision/media/erode.png
    :width: 120px
    :align: center
    :height: 60px
    :alt: Erosion Example
    :figclass: align-center

.. tabs::

   .. code-tab:: java

       Imgproc.erode(src, dst, kernel);

   .. code-tab:: c++

       cv::erode(src, dst, kernel, Point(-1,-1), 1);

   .. code-tab:: py

       erosion = cv2.erode(src, kernel, iterations=1)

Dilation
------------

The dilation of :math:`A` by the structuring element :math:`B` is defined by:

:math:`A\oplus B=\bigcup _{{b\in B}}A_{b}.`

.. figure:: ../vision/media/dilate.png
    :width: 120px
    :align: center
    :height: 60px
    :alt: Dilation Example
    :figclass: align-center

.. tabs::

   .. code-tab:: java

       Imgproc.dilate(src, dst, kernel);

   .. code-tab:: c++

       cv::dilate(src, dst, kernel, Point(-1,-1), 1);

   .. code-tab:: py

       dilation = cv2.dilate(src, kernel, iterations=1)

Properties of Morphologpical Operations
---------------------------------------

* They are translation invariant.
* Dilation is associative.
* Dilation is distributive over set union.
* Erosion is distributive over set intersection.
* Dilation is a pseudo-inverse of the erosion, and vice versa.

While understanding the set theory axioms of erosion and dilation is not necessary to understand them, they are still interesting.

Morophological Operations Playground
------------------------------------

Open
~~~~

Open is another name of erosion followed by dilation. It is useful in removing noise.

.. figure:: ../vision/media/open.png
    :width: 200px
    :align: center
    :height: 100px
    :alt: Open Example
    :figclass: align-center

.. tabs::

   .. code-tab:: java

       Imgproc.morphologyEx(mFGMask,mFGMask,Imgproc.MORPH_OPEN,kernel);    

   .. code-tab:: c++

       cv::morphologyEx(src, dst, MORPH_OPEN, kernel, Point(-1,-1), 1);  

   .. code-tab:: py

       opening = cv2.morphologyEx(src, cv2.MORPH_OPEN, kernel)


Close
~~~~~

Closing is reverse of Opening, Dilation followed by Erosion. It is useful in closing small holes inside the foreground objects.

.. figure:: ../vision/media/close.png
    :width: 200px
    :align: center
    :height: 100px
    :alt: Close Example
    :figclass: align-center

.. tabs::

   .. code-tab:: java

       Imgproc.morphologyEx(mFGMask,mFGMask,Imgproc.MORPH_CLOSE,kernel);

   .. code-tab:: c++

       cv::morphologyEx(src, dst, MORPH_CLOSE, kernel, Point(-1,-1), 1);  

   .. code-tab:: py

       closing = cv2.morphologyEx(src, cv2.MORPH_CLOSE, kernel)

Morphological Gradient
~~~~~~~~~~~~~~~~~~~~~~

It is the difference between dilation and erosion of an image. The result will look like the outline of the object.


.. figure:: ../vision/media/gradient.png
    :width: 200px
    :align: center
    :height: 100px
    :alt: Gradient Example
    :figclass: align-center

.. tabs::

   .. code-tab:: java

        Imgproc.morphologyEx(mFGMask,mFGMask,Imgproc.MORPH_GRADIENT,kernel);
       
   .. code-tab:: c++

       cv::morphologyEx(src, dst, MORPH_GRADIENT, kernel, Point(-1,-1), 1);   

   .. code-tab:: py

       opening = cv2.morphologyEx(src, cv2.MORPH_GRADIENT, kernel)

Uses in FRC
-----------

FRC provides less than ideal environments for computer vision. Often times there is noise in your images that cannot be overcome by reducing the exposure of your camera and thresholding. When this occurs, consider using a morphological operation.

In many cases, however, these standard kernels will not suffice for FRC. Either erosion will remove too much of the target(s) or dilate will combine targets.

The following kernel will not alter the width of a pixel cluster, only the height. This is useful for when your targets are close together horizontally or if the target is not very tall and you must erode.

:math:`\left( \begin{array}{ccc} 0 & 0 & 0 \\ 1 & 1 & 1 \\ 0 & 0 & 0 \end{array} \right)`

.. figure:: ../vision/media/horizontal.png
    :width: 200px
    :align: center
    :height: 100px
    :alt: Gradient Example
    :figclass: align-center

What is not pictured is the border on the top and left side of the image. this is a consequence of this kernel. Despite this consequence, this kernel may be very useful in many cases.

Likewise, this kernel will not alter the height of the pixel cluster, only the width, this is useful with targets who are close together vertically or if the target is not vey tall and you must erode.

:math:`\left( \begin{array}{ccc} 0 & 1 & 0 \\ 0 & 1 & 0 \\ 0 & 1 & 0 \end{array} \right)`

.. figure:: ../vision/media/verticle.png
    :width: 200px
    :align: center
    :height: 100px
    :alt: Gradient Example
    :figclass: align-center
