.. image:: docs/source/_static/logo.png
    :align: left
    
-----------

.. image:: https://travis-ci.org/rshkarin/quanfima.svg?branch=master 
    :target: https://travis-ci.org/rshkarin/quanfima 
    
.. image:: https://readthedocs.org/projects/quanfima/badge/?version=latest 
    :target: http://quanfima.readthedocs.io/en/latest/?badge=latest 
    :alt: Documentation Status
    
.. image:: https://zenodo.org/badge/127795855.svg
   :target: https://zenodo.org/badge/latestdoi/127795855


*Quanfima* (**qu**\ antitative **an**\ alysis of **fi**\ brous **ma**\ terials)
is a collection of useful functions for morphological analysis and visualization
of 2D/3D data from various areas of material science. The aim is to simplify
the analysis process by providing functionality for frequently required tasks
in the same place.

More examples of usage you can find in the documentation.

- Analysis of fibrous structures by tensor-based method in 2D / 3D datasets.
- Estimation of structure diameters in 2D / 3D by a ray-casting method.
- Counting of particles in 2D / 3D datasets and providing a detailed report in
  pandas.DataFrame format.
- Calculation of porosity measure for each material in 2D / 3D datasets.
- Visualization in 2D / 3D using matplotlib, visvis packages.



*IMPORTANT NOTICE*
------------------

This is a fork of the original `Quanfima package <https://github.com/rshkarin/quanfima.git>`_ that has been modified to work with more updated packages. However, because of how complicated vigra is to install, I removed the dependency on vigra and hence, the functions that used that package are no longer available. Some other functionalities might have been affected by the updated packages and the removal of vigra. 

Installation
------------

Because of the *peculiar* versionning of the different packages, I *strongly* recommend to use a virtual environment and install the package::

    $ git clone https://github.com/valentinlemaire/quanfima.git
    $ cd quanfima
    $ virtualenv venv
    $ source venv/bin/activate
    $ python setup.py install

Usage
-----
Open a grayscale image, perform segmentation, estimate porosity, analyze fiber
orientation and diameters, and plot the results.

.. code-block:: python

  import numpy as np
  from skimage import io, filters
  from quanfima import morphology as mrph
  from quanfima import visualization as vis
  from quanfima import utils

  img = io.imread('../data/polymer_slice.tif')

  th_val = filters.threshold_otsu(img)
  img_seg = (img > th_val).astype(np.uint8)

  # estimate porosity
  pr = mrph.calc_porosity(img_seg)
  for k,v in pr.items():
    print('Porosity ({}): {}').format(k, v)

  # prepare data and analyze fibers
  data, skeleton, skeleton_thick = utils.prepare_data(img_seg)
  cskel, fskel, omap, dmap, ovals, dvals = \
                      mrph.estimate_fiber_properties(data, skeleton)

  # plot results
  vis.plot_orientation_map(omap, fskel, min_label=u'0°', max_label=u'180°',
                           figsize=(10,10),
                           name='2d_polymer')
  vis.plot_diameter_map(dmap, cskel, figsize=(10,10), cmap='gist_rainbow',
                        name='2d_polymer')

Documentation 
-------------

For more information about the package, please visit the orignial `repository <https://github.com/rshkarin/quanfima.git>`_ or the `documentation <http://quanfima.readthedocs.io/en/latest/>`_.

