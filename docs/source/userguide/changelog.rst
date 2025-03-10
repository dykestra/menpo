Changelog
#########

0.7.0 (2016/03/21)
------------------
New release that contains some minor breaking changes. In general, the biggest
two changes are:

  - Use ImageIO rather than Pillow for basic importing of some image types.
    The most important aspect of this change is that we now support importing
    videos! Our GIF support also became much more robust.
  - Change multi-asset importing to use a new type - the ``LazyList``. Lazy
    lists are a generic concept for a container that holds onto a list of
    callables which are invoked on indexing. This means that image importing,
    for example, returns immediately but can be **randomly indexed**. This is
    in contrast to generators, which have to be sequentially accessed. This
    is particularly important for video support, as the frames can be accessed
    randomly or sliced from the end (rather than having to pay the penalty
    of importing the entirety of a long video just to access the last frame,
    for example).

These changes should not affect code currently in production - since these are
behavioural breaking changes rather than syntax. Note that this release
also officially supports Python 3.5!

Breaking Changes
................

  - ImageIO is used for importing. Therefore, the pixel values of some images
    have changed due to the difference in underlying importing code.
  - Multi-asset importers are now ``LazyList``s.
  - HOG previously returned negative values due to rounding errors on binning.
    This has been rectified, so the output values of HOG are now slightly
    different.

New Features
............

  - ``from_tri_mask`` method added to ``TriMesh``
  - ``LazyList`` type that holds a list of callables that are invoked on
    indexing.
  - New rasterize methods. Given an image and a landmark group, return a new
    image with the landmarks rasterized onto the image. Useful for saving
    results to disk.
  - Python 3.5 support!
  - Better support for non ``float64`` image types. For example,
    ``as_greyscale`` can be called on a ``uint8`` image.

Deprecated
..........

  - The previously deprecated ``inplace`` image methods **were not removed
    in this release**.
  - ``set_h_matrix`` is deprecated for ``Homogeneous`` transforms.

Github Pull Requests
....................

- `#682`_ Update the view_patches to show only the selected landmarks. (@grigorisg9gr)
- `#639`_ add from_tri_mask method to TriMesh instances (@patricksnape, @jabooth)
- `#680`_ Expose file extension to exporters (Fix PIL exporter bug) (@patricksnape)
- `#672`_ Use Conda environment.yml on RTD (@patricksnape)
- `#678`_ Deprecate set_h_matrix and fix #677 (@patricksnape)
- `#676`_ Implement LazyList __add__ (@patricksnape)
- `#633`_ BREAKING: Imageio (@patricksnape)
- `#648`_ Try turning coverage checking back on (@patricksnape)
- `#670`_ Rasterize 2D Landmarks Method (@patricksnape)
- `#606`_ Fix negative values in HOG calculation (@patricksnape)
- `#673`_ Fix the widgets in PCA. (@grigorisg9gr)
- `#669`_ BREAKING: Add LazyList - default importing is now Lazy (@patricksnape)
- `#668`_ Speedup as_greyscale (@patricksnape)
- `#666`_ Add the protocol option in exporting pickle. (@grigorisg9gr)
- `#665`_ Fix bug with patches of different type than float64 (@patricksnape)
- `#664`_ Python 3.5 builds (@patricksnape)
- `#661`_ Return labels - which maps to a KeysView as a list (@patricksnape)

.. _#682: https://github.com/menpo/menpo/pull/682
.. _#639: https://github.com/menpo/menpo/pull/639
.. _#680: https://github.com/menpo/menpo/pull/680
.. _#672: https://github.com/menpo/menpo/pull/672
.. _#678: https://github.com/menpo/menpo/pull/678
.. _#676: https://github.com/menpo/menpo/pull/676
.. _#633: https://github.com/menpo/menpo/pull/633
.. _#648: https://github.com/menpo/menpo/pull/648
.. _#670: https://github.com/menpo/menpo/pull/670
.. _#606: https://github.com/menpo/menpo/pull/606
.. _#673: https://github.com/menpo/menpo/pull/673
.. _#669: https://github.com/menpo/menpo/pull/669
.. _#668: https://github.com/menpo/menpo/pull/668
.. _#666: https://github.com/menpo/menpo/pull/666
.. _#665: https://github.com/menpo/menpo/pull/665
.. _#664: https://github.com/menpo/menpo/pull/664
.. _#661: https://github.com/menpo/menpo/pull/661

0.6.2 (2015/12/13)
------------------
Add axes ticks option to ``view_patches``.

Github Pull Requests
....................

- `#659`_ Add axes ticks options to view_patches (@nontas)
.. _#659: https://github.com/menpo/menpo/pull/659

0.6.1 (2015/12/09)
------------------
Fix a nasty bug pertaining to a Diamond inheritance problem in PCA. Add the
Gaussion Markov Random Field (GRMF) model. Also a couple of other
bugfixes for visualization.

Github Pull Requests
....................

- `#658`_ PCA Diamond problem fix (@patricksnape)
- `#655`_ Bugfix and improvements in visualize package (@nontas)
- `#656`_ print_dynamic bugfix (@nontas)
- `#635`_ Gaussian Markov Random Field (@nontas, @patricksnape)

.. _#658: https://github.com/menpo/menpo/pull/658
.. _#655: https://github.com/menpo/menpo/pull/655
.. _#656: https://github.com/menpo/menpo/pull/656
.. _#635: https://github.com/menpo/menpo/pull/635

0.6.0 (2015/11/26)
------------------
This release is another set of breaking changes for Menpo. All ``in_place`` 
methods have been deprecated to make the API clearer (always copy). The largest
change is the removal of all widgets into a subpackage called `menpowidgets`_.
To continue using widgets within the Jupyter notebook, you should install
menpowidgets. 

Breaking Changes
................

  - Procrustes analysis now checks for mirroring and disables it by default.
    This is a change in behaviour.
  - The ``sample_offsets`` argument of 
    :func:`menpo.image.Image.extract_patches` now expects a
    numpy array rather than a :map:`PointCloud`.
  - All widgets are removed and now exist as part of the `menpowidgets`_ 
    project. The widgets are now only compatible with Jupyter 4.0 and above.
  - Landmark labellers have been totally refactored and renamed. They have
    not been deprecated due to the changes. However, the new changes mean
    that the naming scheme of labels is now much more intuitive. Practically,
    the usage of labelling has only changed in that now it is possible to label
    not only :map:`LandmarkGroup` but also :map:`PointCloud` and numpy arrays
    directly.
  - Landmarks are now warped by default, where previously they were not.
  - All vlfeat features have now become optional and will not appear if
    cyvlfeat is not installed.
  - All ``label`` keyword arguments have been removed. They were not found
    to be useful. For the same effect, you can always create a new landmark 
    group that only contains that label and use that as the ``group`` key.

New Features
............

  - New SIFT type features that return vectors rather than dense features.
    (:func:`menpo.feature.vector_128_dsift`, 
    :func:`menpo.feature.hellinger_vector_128_dsift`)
  - :func:`menpo.shape.PointCloud.init_2d_grid` static constructor for 
    :map:`PointCloud` and subclasses.
  - Add :map:`PCAVectorModel` class that allows performing PCA directly on 
    arrays.
  - New static constructors on PCA models for building PCA directly from
    covariance matrices or components 
    (:func:`menpo.model.PCAVectorModel.init_from_components` and 
    :func:`menpo.model.PCAVectorModel.init_from_covariance_matrix`).
  - New :func:`menpo.image.Image.mirror` method on images.
  - New :func:`menpo.image.Image.set_patches` methods on images.
  - New :func:`menpo.image.Image.rotate_ccw_about_centre` method on images.
  - When performing operations on images, you can now add the 
    ``return_transform`` kwarg that will return both the new image **and** the
    transform that created the image. This can be very useful for processing
    landmarks after images have been cropped and rescaled for example.

.. _menpowidgets: https://github.com/menpo/menpowidgets

Github Pull Requests
....................
- `#652`_ Deprecate a number of inplace methods (@jabooth)
- `#653`_ New features (vector dsift) (@patricksnape)
- `#651`_ remove deprecations from 0.5.0 (@jabooth)
- `#650`_ PointCloud init_2d_grid (@patricksnape)
- `#646`_ Add ibug_49 -> ibug_49 labelling (@patricksnape)
- `#645`_ Add new PCAVectorModel class, refactor model package (@patricksnape, @nontas)
- `#644`_ Remove label kwarg (@patricksnape)
- `#643`_ Build fixes (@patricksnape)
- `#638`_ bugfix 2D triangle areas sign was ambiguous (@jabooth)
- `#634`_ Fixing @patricksnape and @nontas foolish errors (@yuxiang-zhou)
- `#542`_ Add mirroring check to procrustes (@nontas, @patricksnape)
- `#632`_ Widgets Migration (@patricksnape, @nontas)
- `#631`_ Optional transform return on Image methods (@nontas)
- `#628`_ Patches Visualization (@nontas)
- `#629`_ Image counter-clockwise rotation (@nontas)
- `#630`_ Mirror image (@nontas)
- `#625`_ Labellers Refactoring (@patricksnape)
- `#623`_ Fix widgets for new Jupyter/IPython 4 release (@patricksnape)
- `#620`_ Define patches offsets as ndarray (@nontas)

.. _#652: https://github.com/menpo/menpo/pull/652
.. _#653: https://github.com/menpo/menpo/pull/653
.. _#651: https://github.com/menpo/menpo/pull/651
.. _#650: https://github.com/menpo/menpo/pull/650
.. _#646: https://github.com/menpo/menpo/pull/646
.. _#645: https://github.com/menpo/menpo/pull/645
.. _#644: https://github.com/menpo/menpo/pull/644
.. _#643: https://github.com/menpo/menpo/pull/643
.. _#638: https://github.com/menpo/menpo/pull/638
.. _#634: https://github.com/menpo/menpo/pull/634
.. _#542: https://github.com/menpo/menpo/pull/542
.. _#632: https://github.com/menpo/menpo/pull/632
.. _#631: https://github.com/menpo/menpo/pull/631
.. _#628: https://github.com/menpo/menpo/pull/628
.. _#629: https://github.com/menpo/menpo/pull/629
.. _#630: https://github.com/menpo/menpo/pull/630
.. _#625: https://github.com/menpo/menpo/pull/625
.. _#623: https://github.com/menpo/menpo/pull/623
.. _#620: https://github.com/menpo/menpo/pull/620


0.5.3 (2015/08/12)
------------------
Tiny point release just fixing a typo in the ``unique_edge_indices`` method.

0.5.2 (2015/08/04)
------------------
Minor bug fixes and impovements including:

  - Menpo is now better at preserving dtypes other than np.float through common 
    operations
  - Image has a new convenience constructor ``init_from_rolled_channels()`` to 
    handle building images that have the channels at the back of the array.
  - There are also new ``crop_to_pointcloud()`` and 
    ``crop_to_pointcloud_proportion()`` methods to round out the Image API, 
    and a deprecation of ``rescale_to_reference_shape()`` in favour of 
    ``rescale_to_pointcloud()`` to make things more consistent.
  - The ``gradient()`` method is deprecated (use ``menpo.feature.gradient`` 
    instead)
  - Propagation of the ``.path`` property when using ``as_masked()`` was fixed
  - Fix for exporting 3D LJSON landmark files
  - A new ``shuffle`` kwarg (default ``False``) is present on all multi 
    importers.

Github Pull Requests
....................
- `#617`_ add shuffle kwarg to multi import generators (@jabooth)
- `#619`_ Ensure that LJSON landmarks are read in as floats (@jabooth)
- `#618`_ Small image fix (@patricksnape)
- `#613`_ Balance out rescale/crop methods (@patricksnape)
- `#615`_ Allow exporting of 3D landmarks. (@mmcauliffe)
- `#612`_ Type maintain (@patricksnape)
- `#602`_ Extract patches types (@patricksnape)
- `#608`_ Slider for selecting landmark group on widgets (@nontas)
- `#605`_ tmp move to master condaci (@jabooth)

.. _#617: https://github.com/menpo/menpo/pull/617
.. _#619: https://github.com/menpo/menpo/pull/619
.. _#618: https://github.com/menpo/menpo/pull/618
.. _#613: https://github.com/menpo/menpo/pull/613
.. _#615: https://github.com/menpo/menpo/pull/615
.. _#612: https://github.com/menpo/menpo/pull/612
.. _#602: https://github.com/menpo/menpo/pull/602
.. _#608: https://github.com/menpo/menpo/pull/608
.. _#605: https://github.com/menpo/menpo/pull/605

0.5.1 (2015/07/16)
------------------
A small point release that improves the Cython code (particularly
extracting patches) compatibility with different data types. In particular,
more floating point data types are now supported. ``print_progress``
was added and widgets were fixed after the Jupyter 4.0 release. Also,
upgrade cyvlfeat requirement to 0.4.0.

Github Pull Requests
....................
- `#604`_ print_progress enhancements (@jabooth)
- `#603`_ Fixes for new cyvlfeat (@patricksnape)
- `#599`_ Add erode and dilate methods to MaskedImage (@jalabort)
- `#601`_ Add sudo: false to turn on container builds (@patricksnape)
- `#600`_ Human3.6M labels (@nontas)

.. _#604: https://github.com/menpo/menpo/pull/604
.. _#603: https://github.com/menpo/menpo/pull/603
.. _#599: https://github.com/menpo/menpo/pull/599
.. _#601: https://github.com/menpo/menpo/pull/601
.. _#600: https://github.com/menpo/menpo/pull/600

0.5.0 (2015/06/25)
------------------
This release of Menpo makes a number of very important **BREAKING** changes
to the format of Menpo's core data types. Most importantly is `#524`_ which
swaps the position of the channels on an image from the last axis to the first.
This is to maintain row-major ordering and make iterating over the pixels
of a channel efficient. This made a huge improvement in speed in other packages
such as MenpoFit. It also makes common operations such as iterating over
the pixels in an image much simpler:

.. code-block:: python

    for channels in image.pixels:
        print(channels.shape)  # This will be a (height x width) ndarray

Other important changes include:

  - Updating all widgets to work with IPython 3
  - Incremental PCA was added.
  - non-inplace cropping methods
  - Dense SIFT features provided by vlfeat
  - The implementation of graphs was changed to use sparse matrices by default.
    **This may cause breaking changes.**
  - Many other improvements detailed in the pull requests below!

If you have serialized data using Menpo, you will likely find you have trouble
reimporting it. If this is the case, please visit the user group for advice.

Github Pull Requests
....................
- `#598`_  Visualize sum of channels in widgets (@nontas, @patricksnape)
- `#597`_  test new dev tag behavior on condaci (@jabooth)
- `#591`_  Scale around centre (@patricksnape)
- `#596`_  Update to versioneer v0.15 (@jabooth, @patricksnape)
- `#495`_  SIFT features (@nontas, @patricksnape, @jabooth, @jalabort)
- `#595`_  Update mean_pointcloud (@patricksnape, @jalabort)
- `#541`_  Add triangulation labels for ibug_face_(66/51/49) (@jalabort)
- `#590`_  Fix centre and diagonal being properties on Images (@patricksnape)
- `#592`_  Refactor out bounding_box method (@patricksnape)
- `#566`_  TriMesh utilities (@jabooth)
- `#593`_  Minor bugfix on AnimationOptionsWidget (@nontas)
- `#587`_  promote non-inplace crop methods, crop performance improvements (@jabooth, @patricksnape)
- `#586`_  fix as_matrix where the iterator finished early (@jabooth)
- `#574`_  Widgets for IPython3 (@nontas, @patricksnape, @jabooth)
- `#588`_  test condaci 0.2.1, less noisy slack notifications (@jabooth)
- `#568`_  rescale_pixels() for rescaling the range of pixels (@jabooth)
- `#585`_  Hotfix: suffix change led to double path resolution. (@patricksnape)
- `#581`_  Fix the landmark importer in case the landmark file has a '.' in its filename. (@grigorisg9gr)
- `#584`_  new print_progress visualization function (@jabooth)
- `#580`_  export_pickle now ensures pathlib.Path save as PurePath (@jabooth)
- `#582`_  New readers for Middlebury FLO and FRGC ABS files (@patricksnape)
- `#579`_  Fix the image importer in case of upper case letters in the suffix (@grigorisg9gr)
- `#575`_  Allowing expanding user paths in exporting pickle (@patricksnape)
- `#577`_  Change to using run_test.py (@patricksnape)
- `#570`_  Zoom (@jabooth, @patricksnape)
- `#569`_  Add new point_in_pointcloud kwarg to constrain (@patricksnape)
- `#563`_  TPS Updates (@patricksnape)
- `#567`_  Optional cmaps (@jalabort)
- `#559`_  Graphs with isolated vertices (@nontas)
- `#564`_  Bugfix: PCAModel print (@nontas)
- `#565`_  fixed minor typo in introduction.rst (@evanjbowling)
- `#562`_  IPython3 widgets (@patricksnape, @jalabort)
- `#558`_  Channel roll (@patricksnape)
- `#524`_  BREAKING CHANGE: Channels flip (@patricksnape, @jabooth, @jalabort)
- `#512`_  WIP: remove_all_landmarks convienience method, quick lm filter (@jabooth)
- `#554`_  Bugfix:visualize_images (@nontas)
- `#553`_  Transform docs fixes (@nontas)
- `#533`_  LandmarkGroup.init_with_all_label, init_* convenience constructors (@jabooth, @patricksnape)
- `#552`_  Many fixes for Python 3 support (@patricksnape)
- `#532`_  Incremental PCA (@patricksnape, @jabooth, @jalabort)
- `#528`_  New as_matrix and from_matrix methods (@patricksnape)

.. _#598: https://github.com/menpo/menpo/pull/598
.. _#597: https://github.com/menpo/menpo/pull/597
.. _#591: https://github.com/menpo/menpo/pull/591
.. _#596: https://github.com/menpo/menpo/pull/596
.. _#495: https://github.com/menpo/menpo/pull/495
.. _#595: https://github.com/menpo/menpo/pull/595
.. _#541: https://github.com/menpo/menpo/pull/541
.. _#590: https://github.com/menpo/menpo/pull/590
.. _#592: https://github.com/menpo/menpo/pull/592
.. _#566: https://github.com/menpo/menpo/pull/566
.. _#593: https://github.com/menpo/menpo/pull/593
.. _#587: https://github.com/menpo/menpo/pull/587
.. _#586: https://github.com/menpo/menpo/pull/586
.. _#574: https://github.com/menpo/menpo/pull/574
.. _#588: https://github.com/menpo/menpo/pull/588
.. _#568: https://github.com/menpo/menpo/pull/568
.. _#585: https://github.com/menpo/menpo/pull/585
.. _#581: https://github.com/menpo/menpo/pull/581
.. _#584: https://github.com/menpo/menpo/pull/584
.. _#580: https://github.com/menpo/menpo/pull/580
.. _#582: https://github.com/menpo/menpo/pull/582
.. _#579: https://github.com/menpo/menpo/pull/579
.. _#575: https://github.com/menpo/menpo/pull/575
.. _#577: https://github.com/menpo/menpo/pull/577
.. _#570: https://github.com/menpo/menpo/pull/570
.. _#569: https://github.com/menpo/menpo/pull/569
.. _#563: https://github.com/menpo/menpo/pull/563
.. _#567: https://github.com/menpo/menpo/pull/567
.. _#559: https://github.com/menpo/menpo/pull/559
.. _#564: https://github.com/menpo/menpo/pull/564
.. _#565: https://github.com/menpo/menpo/pull/565
.. _#562: https://github.com/menpo/menpo/pull/562
.. _#524: https://github.com/menpo/menpo/pull/524
.. _#512: https://github.com/menpo/menpo/pull/512
.. _#554: https://github.com/menpo/menpo/pull/554
.. _#553: https://github.com/menpo/menpo/pull/553
.. _#533: https://github.com/menpo/menpo/pull/533
.. _#552: https://github.com/menpo/menpo/pull/552
.. _#532: https://github.com/menpo/menpo/pull/532
.. _#528: https://github.com/menpo/menpo/pull/528
.. _#558: https://github.com/menpo/menpo/pull/558


0.4.4 (2015/03/05)
------------------
A hotfix release for properly handling nan values in the landmark formats. Also,
a few other bug fixes crept in:

 - Fix 3D Ljson importing
 - Fix trim_components on PCA
 - Fix setting None key on the landmark manager
 - Making mean_pointcloud faster

Also makes an important change to the build configuration that syncs this
version of Menpo to IPython 2.x.

Github Pull Requests
....................
- `#560`_  Assorted fixes (@patricksnape)
- `#557`_  Ljson nan fix (@patricksnape)

.. _#560: https://github.com/menpo/menpo/pull/560
.. _#557: https://github.com/menpo/menpo/pull/557


0.4.3 (2015/02/19)
------------------
Adds the concept of nan values to the landmarker format for labelling missing
landmarks.

Github Pull Requests
....................
- `#556`_  [0.4.x] Ljson nan/null fixes (@patricksnape)

.. _#556: https://github.com/menpo/menpo/pull/556

0.4.2 (2015/02/19)
------------------
A hotfix release for landmark groups that have no connectivity.

Github Pull Requests
....................
- `#555`_  don't try and build a Graph with no connectivity (@jabooth)

.. _#555: https://github.com/menpo/menpo/pull/555

0.4.1 (2015/02/07)
------------------
A hotfix release to enable compatibility with landmarker.io.

Github Pull Requests
....................
- `#551`_  HOTFIX: remove incorrect tojson() methods (@jabooth)

.. _#551: https://github.com/menpo/menpo/pull/551

0.4.0 (2015/02/04)
------------------
The 0.4.0 release (pending any currently unknown bugs), represents a very
significant overhaul of Menpo from v0.3.0. In particular, Menpo has been
broken into four distinct packages: Menpo, MenpoFit, Menpo3D and MenpoDetect.

Visualization has had major improvements for 2D viewing, in particular
through the use of IPython widgets and explicit options on the viewing methods
for common tasks (like changing the landmark marker color). This final release
is a much smaller set of changes over the alpha releases, so please check the
full changelog for the alphas to see all changes from v0.3.0 to v0.4.0.

**Summary of changes since v0.4.0a2**:

  - Lots of documentation rendering fixes and style fixes including this
    changelog.
  - Move the LJSON format to V2. V1 is now being deprecated over the next
    version.
  - More visualization customization fixes including multiple marker colors
    for landmark groups.

Github Pull Requests
....................
- `#546`_ IO doc fixes (@jabooth)
- `#545`_ Different marker colour per label (@nontas)
- `#543`_ Bug fix for importing an image, case of a dot in image name. (@grigorisg9gr)
- `#544`_ Move docs to Sphinx 1.3b2 (@patricksnape)
- `#536`_ Docs fixes (@patricksnape)
- `#530`_ Visualization and Widgets upgrade (@patricksnape, @nontas)
- `#540`_ LJSON v2 (@jabooth)
- `#537`_ fix BU3DFE connectivity, pretty JSON files (@jabooth)
- `#529`_ BU3D-FE labeller added (@jabooth)
- `#527`_ fixes paths for pickle importing (@jabooth)
- `#525`_ Fix .rst doc files, auto-generation script (@jabooth)

.. _#546: https://github.com/menpo/menpo/pull/546
.. _#545: https://github.com/menpo/menpo/pull/545
.. _#544: https://github.com/menpo/menpo/pull/544
.. _#543: https://github.com/menpo/menpo/pull/543
.. _#540: https://github.com/menpo/menpo/pull/540
.. _#536: https://github.com/menpo/menpo/pull/536
.. _#537: https://github.com/menpo/menpo/pull/537
.. _#530: https://github.com/menpo/menpo/pull/530
.. _#529: https://github.com/menpo/menpo/pull/529
.. _#527: https://github.com/menpo/menpo/pull/527
.. _#525: https://github.com/menpo/menpo/pull/525

v0.4.0a2 (2014/12/03)
---------------------
Alpha 2 moves towards extending the graphing API so that visualization is
more dependable.

**Summary:**

  - Add graph classes, :map:`PointUndirectedGraph`, :map:`PointDirectedGraph`,
    :map:`PointTree`. This makes visualization of landmarks much nicer looking.
  - Better support of pickling menpo objects
  - Add a bounding box method to :map:`PointCloud` for calculating the correctly
    oriented bounding box of point clouds.
  - Allow PCA to operate in place for large data matrices.

Github Pull Requests
....................
- `#522`_ Add bounding box method to pointclouds (@patricksnape)
- `#523`_ HOTFIX: fix export_pickle bug, add path support (@jabooth)
- `#521`_ menpo.io add pickle support, move to pathlib (@jabooth)
- `#520`_ Documentation fixes (@patricksnape, @jabooth)
- `#518`_ PCA memory improvements, inplace dot product (@jabooth)
- `#519`_ replace wrapt with functools.wraps - we can pickle (@jabooth)
- `#517`_ (@jabooth)
- `#514`_ Remove the use of triplot (@patricksnape)
- `#516`_ Fix how images are converted to PIL (@patricksnape)
- `#515`_ Show the path in the image widgets (@patricksnape)
- `#511`_ 2D Rotation convenience constructor, Image.rotate_ccw_about_centre (@jabooth)
- `#510`_ all menpo io glob operations are now always sorted (@jabooth)
- `#508`_ visualize image on MaskedImage reports Mask proportion (@jabooth)
- `#509`_ path is now preserved on image warping (@jabooth)
- `#507`_ fix rounding issue in n_components (@jabooth)
- `#506`_ is_tree update in Graph (@nontas)
- `#505`_ (@nontas)
- `#504`_ explicitly have kwarg in IO for landmark extensions (@jabooth)
- `#503`_ Update the README (@patricksnape)

.. _#523: https://github.com/menpo/menpo/pull/523
.. _#522: https://github.com/menpo/menpo/pull/522
.. _#521: https://github.com/menpo/menpo/pull/521
.. _#520: https://github.com/menpo/menpo/pull/520
.. _#519: https://github.com/menpo/menpo/pull/519
.. _#518: https://github.com/menpo/menpo/pull/518
.. _#517: https://github.com/menpo/menpo/pull/517
.. _#516: https://github.com/menpo/menpo/pull/516
.. _#515: https://github.com/menpo/menpo/pull/515
.. _#514: https://github.com/menpo/menpo/pull/514
.. _#511: https://github.com/menpo/menpo/pull/511
.. _#510: https://github.com/menpo/menpo/pull/510
.. _#509: https://github.com/menpo/menpo/pull/509
.. _#508: https://github.com/menpo/menpo/pull/508
.. _#507: https://github.com/menpo/menpo/pull/507
.. _#506: https://github.com/menpo/menpo/pull/506
.. _#505: https://github.com/menpo/menpo/pull/505
.. _#504: https://github.com/menpo/menpo/pull/504
.. _#503: https://github.com/menpo/menpo/pull/503

v0.4.0a1 (2014/10/31)
---------------------
This first alpha release makes a number of large, breaking changes to Menpo
from v0.3.0. The biggest change is that Menpo3D and MenpoFit were created
and thus all AAM and 3D visualization/rasterization code has been moved out
of the main Menpo repository. This is working towards Menpo being pip
installable.

**Summary:**

  - Fixes memory leak whereby weak references were being kept between
    landmarks and their host objects. The Landmark manager now no longer
    keeps references to its host object. This also helps with serialization.
  - Use pathlib instead of strings for paths in the ``io`` module.
  - Importing of builtin assets from a simple function
  - Improve support for image importing (including ability to import without
    normalising)
  - Add fast methods for image warping, ``warp_to_mask`` and ``warp_to_shape``
    instead of ``warp_to``
  - Allow masking of triangle meshes
  - Add IPython visualization widgets for our core types
  - All expensive properties (properties that would be worth caching in
    a variable and are not merely a lookup) are changed to methods.

Github Pull Requests
....................
- `#502`_ Fixes pseudoinverse for Alignment Transforms (@jalabort, @patricksnape)
- `#501`_ Remove menpofit widgets (@nontas)
- `#500`_ Shapes widget (@nontas)
- `#499`_ spin out AAM, CLM, SDM, ATM and related code to menpofit (@jabooth)
- `#498`_ Minimum spanning tree bug fix (@nontas)
- `#492`_ Some fixes for PIL image importing (@patricksnape)
- `#494`_ Widgets bug fix and Active Template Model widget (@nontas)
- `#491`_ Widgets fixes (@nontas)
- `#489`_ remove _view, fix up color_list -> colour_list (@jabooth)
- `#486`_ Image visualisation improvements (@patricksnape)
- `#488`_ Move expensive image properties to methods (@jabooth)
- `#487`_ Change expensive PCA properties to methods (@jabooth)
- `#485`_ MeanInstanceLinearModel.mean is now a method (@jabooth)
- `#452`_ Advanced widgets (@patricksnape, @nontas)
- `#481`_ Remove 3D (@patricksnape)
- `#480`_ Graphs functionality (@nontas)
- `#479`_ Extract patches on image (@patricksnape)
- `#469`_ Active Template Models (@nontas)
- `#478`_ Fix residuals for AAMs (@patricksnape, @jabooth)
- `#474`_ remove HDF5able making room for h5it (@jabooth)
- `#475`_ Normalize norm and std of Image object (@nontas)
- `#472`_ Daisy features (@nontas)
- `#473`_ Fix from_mask for Trimesh subclasses (@patricksnape)
- `#470`_ expensive properties should really be methods (@jabooth)
- `#467`_ get a progress bar on top level feature computation (@jabooth)
- `#466`_ Spin out rasterization and related methods to menpo3d (@jabooth)
- `#465`_ 'me_norm' error type in tests (@nontas)
- `#463`_ goodbye ioinfo, hello path (@jabooth)
- `#464`_ make mayavi an optional dependency (@jabooth)
- `#447`_ Displacements in fitting result (@nontas)
- `#451`_ AppVeyor Windows continuous builds from condaci (@jabooth)
- `#445`_ Serialize fit results (@patricksnape)
- `#444`_ remove pyramid_on_features from Menpo (@jabooth)
- `#443`_ create_pyramid now applies features even if pyramid_on_features=False, SDM uses it too (@jabooth)
- `#369`_ warp_to_mask, warp_to_shape, fast resizing of images (@nontas, @patricksnape, @jabooth)
- `#442`_ add rescale_to_diagonal, diagonal property to Image (@jabooth)
- `#441`_ adds constrain_to_landmarks on BooleanImage (@jabooth)
- `#440`_ pathlib.Path can no be used in menpo.io (@jabooth)
- `#439`_ Labelling fixes (@jabooth, @patricksnape)
- `#438`_ extract_channels (@jabooth)
- `#437`_ GLRasterizer becomes HDF5able (@jabooth)
- `#435`_ import_builtin_asset.ASSET_NAME (@jabooth)
- `#434`_ check_regression_features unified with check_features, classmethods removed from SDM (@jabooth)
- `#433`_ tidy classifiers (@jabooth)
- `#432`_ aam.fitter, clm.fitter, sdm.trainer packages (@jabooth)
- `#431`_ More fitmultilevel tidying (@jabooth)
- `#430`_ Remove classmethods from DeformableModelBuilder (@jabooth)
- `#412`_ First visualization widgets (@jalabort, @nontas)
- `#429`_ Masked image fixes (@patricksnape)
- `#426`_ rename 'feature_type' to 'features throughout Menpo (@jabooth)
- `#427`_ Adds HDF5able serialization support to Menpo (@jabooth)
- `#425`_ Faster cached piecewise affine, Cython varient demoted (@jabooth)
- `#424`_ (@nontas)
- `#378`_ Fitting result fixes (@jabooth, @nontas, @jalabort)
- `#423`_ name now displays on constrained features (@jabooth)
- `#421`_ Travis CI now makes builds, Linux/OS X Python 2.7/3.4 (@jabooth, @patricksnape)
- `#400`_ Features as functions (@nontas, @patricksnape, @jabooth)
- `#420`_ move IOInfo to use pathlib (@jabooth)
- `#405`_ import menpo is now twice as fast (@jabooth)
- `#416`_ waffle.io Badge (@waffle-iron)
- `#415`_ export_mesh with .OBJ exporter (@jabooth, @patricksnape)
- `#410`_ Fix the render_labels logic (@patricksnape)
- `#407`_ Exporters (@patricksnape)
- `#406`_ Fix greyscale PIL images (@patricksnape)
- `#404`_ LandmarkGroup tojson method and PointGraph (@patricksnape)
- `#403`_ Fixes a couple of viewing problems in fitting results (@patricksnape)
- `#402`_ Landmarks fixes (@jabooth, @patricksnape)
- `#401`_ Dogfood landmark_resolver in menpo.io (@jabooth)
- `#399`_ bunch of Python 3 compatibility fixes (@jabooth)
- `#398`_ throughout Menpo. (@jabooth)
- `#397`_ Performance improvements for Similarity family (@jabooth)
- `#396`_ More efficient initialisations of Menpo types (@jabooth)
- `#395`_ remove cyclic target reference from landmarks (@jabooth)
- `#393`_ Groundwork for dense correspondence pipeline (@jabooth)
- `#394`_ weakref to break cyclic references (@jabooth)
- `#389`_ assorted fixes (@jabooth)
- `#390`_ (@jabooth)
- `#387`_ Adds landmark label for tongues (@nontas)
- `#386`_ Adds labels for the ibug eye annotation scheme (@jalabort)
- `#382`_ BUG fixed: block element not reset if norm=0 (@dubzzz)
- `#381`_ Recursive globbing (@jabooth)
- `#384`_ Adds support for odd patch shapes in function extract_local_patches_fast (@jalabort)
- `#379`_ imported textures have ioinfo, docs improvements (@jabooth)

.. _#501: https://github.com/menpo/menpo/pull/501
.. _#500: https://github.com/menpo/menpo/pull/500
.. _#499: https://github.com/menpo/menpo/pull/499
.. _#498: https://github.com/menpo/menpo/pull/498
.. _#492: https://github.com/menpo/menpo/pull/492
.. _#494: https://github.com/menpo/menpo/pull/494
.. _#491: https://github.com/menpo/menpo/pull/491
.. _#489: https://github.com/menpo/menpo/pull/489
.. _#486: https://github.com/menpo/menpo/pull/486
.. _#488: https://github.com/menpo/menpo/pull/488
.. _#487: https://github.com/menpo/menpo/pull/487
.. _#485: https://github.com/menpo/menpo/pull/485
.. _#452: https://github.com/menpo/menpo/pull/452
.. _#481: https://github.com/menpo/menpo/pull/481
.. _#480: https://github.com/menpo/menpo/pull/480
.. _#479: https://github.com/menpo/menpo/pull/479
.. _#469: https://github.com/menpo/menpo/pull/469
.. _#478: https://github.com/menpo/menpo/pull/478
.. _#474: https://github.com/menpo/menpo/pull/474
.. _#475: https://github.com/menpo/menpo/pull/475
.. _#472: https://github.com/menpo/menpo/pull/472
.. _#473: https://github.com/menpo/menpo/pull/473
.. _#470: https://github.com/menpo/menpo/pull/470
.. _#467: https://github.com/menpo/menpo/pull/467
.. _#466: https://github.com/menpo/menpo/pull/466
.. _#465: https://github.com/menpo/menpo/pull/465
.. _#463: https://github.com/menpo/menpo/pull/463
.. _#464: https://github.com/menpo/menpo/pull/464
.. _#447: https://github.com/menpo/menpo/pull/447
.. _#451: https://github.com/menpo/menpo/pull/451
.. _#445: https://github.com/menpo/menpo/pull/445
.. _#444: https://github.com/menpo/menpo/pull/444
.. _#443: https://github.com/menpo/menpo/pull/443
.. _#369: https://github.com/menpo/menpo/pull/369
.. _#442: https://github.com/menpo/menpo/pull/442
.. _#441: https://github.com/menpo/menpo/pull/441
.. _#440: https://github.com/menpo/menpo/pull/440
.. _#439: https://github.com/menpo/menpo/pull/439
.. _#438: https://github.com/menpo/menpo/pull/438
.. _#437: https://github.com/menpo/menpo/pull/437
.. _#435: https://github.com/menpo/menpo/pull/435
.. _#434: https://github.com/menpo/menpo/pull/434
.. _#433: https://github.com/menpo/menpo/pull/433
.. _#432: https://github.com/menpo/menpo/pull/432
.. _#431: https://github.com/menpo/menpo/pull/431
.. _#430: https://github.com/menpo/menpo/pull/430
.. _#412: https://github.com/menpo/menpo/pull/412
.. _#429: https://github.com/menpo/menpo/pull/429
.. _#426: https://github.com/menpo/menpo/pull/426
.. _#427: https://github.com/menpo/menpo/pull/427
.. _#425: https://github.com/menpo/menpo/pull/425
.. _#424: https://github.com/menpo/menpo/pull/424
.. _#378: https://github.com/menpo/menpo/pull/378
.. _#423: https://github.com/menpo/menpo/pull/423
.. _#421: https://github.com/menpo/menpo/pull/421
.. _#400: https://github.com/menpo/menpo/pull/400
.. _#420: https://github.com/menpo/menpo/pull/420
.. _#405: https://github.com/menpo/menpo/pull/405
.. _#416: https://github.com/menpo/menpo/pull/416
.. _#415: https://github.com/menpo/menpo/pull/415
.. _#410: https://github.com/menpo/menpo/pull/410
.. _#407: https://github.com/menpo/menpo/pull/407
.. _#406: https://github.com/menpo/menpo/pull/406
.. _#404: https://github.com/menpo/menpo/pull/404
.. _#403: https://github.com/menpo/menpo/pull/403
.. _#402: https://github.com/menpo/menpo/pull/402
.. _#401: https://github.com/menpo/menpo/pull/401
.. _#399: https://github.com/menpo/menpo/pull/399
.. _#398: https://github.com/menpo/menpo/pull/398
.. _#397: https://github.com/menpo/menpo/pull/397
.. _#396: https://github.com/menpo/menpo/pull/396
.. _#395: https://github.com/menpo/menpo/pull/395
.. _#393: https://github.com/menpo/menpo/pull/393
.. _#394: https://github.com/menpo/menpo/pull/394
.. _#389: https://github.com/menpo/menpo/pull/389
.. _#390: https://github.com/menpo/menpo/pull/390
.. _#387: https://github.com/menpo/menpo/pull/387
.. _#386: https://github.com/menpo/menpo/pull/386
.. _#382: https://github.com/menpo/menpo/pull/382
.. _#381: https://github.com/menpo/menpo/pull/381
.. _#384: https://github.com/menpo/menpo/pull/384
.. _#502: https://github.com/menpo/menpo/pull/502
.. _#379: https://github.com/menpo/menpo/pull/379

v0.3.0 (2014/05/27)
-------------------
First public release of Menpo, this release coincided with submission
to the ACM Multimedia Open Source Software Competition 2014. This provides
the basic scaffolding for Menpo, but it is not advised to use this version
over the improvements in 0.4.0.

Github Pull Requests
....................
- `#377`_ Simple fixes (@patricksnape)
- `#375`_ improvements to importing multiple assets (@jabooth)
- `#374`_ Menpo's User guide (@jabooth)

.. _#377: https://github.com/menpo/menpo/pull/377
.. _#375: https://github.com/menpo/menpo/pull/375
.. _#374: https://github.com/menpo/menpo/pull/374
