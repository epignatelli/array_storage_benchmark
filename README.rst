Fork from https://github.com/mverleg/array_storage_benchmark to target HDF5 method.

Note that the test file has the following differences:
- Any line that uses `jt_dump` has been commented out
- Most of the methods where throwing errors, thus I just benchmarked HDF5 and NPY as a reference.

.. image:: https://user-images.githubusercontent.com/26899347/67759979-5c96a580-fa38-11e9-9d71-f98ec1c4e611.png
.. image:: https://user-images.githubusercontent.com/26899347/67759984-5dc7d280-fa38-11e9-8b37-ac264218f25e.png
.. image:: https://user-images.githubusercontent.com/26899347/67759989-5f919600-fa38-11e9-8b84-8262bffb1633.png
.. image:: https://user-images.githubusercontent.com/26899347/67759994-615b5980-fa38-11e9-9301-53432f11dac6.png


Array storage benchmark
---------------------------------------

Compare the storage speed, retrieval speed and file size for various methods of storing 2D numpy arrays.

Hardware etc
---------------------------------------

The results here are obtained on a normal desktop PC that's several years old and running Ubuntu and has a SSD for storage. You can easily run the benchmarks on your own PC to get more relevant results. You can also apply it to your own data.

Methods
---------------------------------------

=========  =======================  =======  =======  ============  ============  ===============  ==========  ===========================
Name       Description              Fast     Small^   Portability   Ease of use   Human-readable   Flexible%   Notes
=========  =======================  =======  =======  ============  ============  ===============  ==========  ===========================
Csv~       comma separated value    ☐ ☐ ☐    ☐ ☐ ☐    ☒ ☒ ☒         ☒ ☒ ☒         ☒ ☒              ☒ ☐         only 2D
JSON~      js object notation       ☐ ☐ ☐    ☐ ☐ ☐    ☒ ☒ ☐         ☒ ☒ ☐ ++      ☒ ☐              ☒ ☒         any dim, unequal rows
b64Enc     base 64 encoding         ☒ ☒ ☒    ☒ ☐ ☐    ☒ ☒ ☐         ☒ ☒ ☐         ☐ ☐              ☐ ☐         more network, not files
Pickle~    python pickle            ☒ ☒ ☐    ☐ ☐ ☐    ☐ ☐ ☐         ☒ ☒ ☒         ☐ ☐              ☒ ☒         any obj, not backw. comp
Binary~    pure raw data            ☒ ☒ ☒    ☒ ☒ ☐    ☒ ☒ ☒         ☒ ☐ ☐         ☐ ☐              ☐ ☐         dim & type separately
NPY        numpy .npy (no pickle)   ☒ ☒ ☒    ☒ ☒ ☐    ☒ ☐ ☐         ☒ ☒ ☒         ☐ ☐              ☒ ☐         with pickle mode OFF
NPYCompr   numpy .npz               ☒ ☒ ☒    ☒ ☒ ☒    ☒ ☐ ☐         ☒ ☒ ☒         ☐ ☐              ☒ ☐         multiple matrices
PNG        encoded as png image     ☒ ☒ ☐    ☒ ☒ ☒    ☐ ☐ ☐         ☐ ☐ ☐ ++      ☐ ☐              ☐ ☐         only 2D; for fun but works
FortUnf    fortran unformatted      ☒ ☒ ☒    ☒ ☒ ☐    ☒ ☐ ☐         ☒ ☐ ☐ +       ☐ ☐              ☒ ☐         often compiler dependent
MatFile    Matlab .mat file         ☒ ☒ ☒    ☒ ☒ ☐    ☒ ☒ ☐         ☒ ☒ ☒ +       ☐ ☐              ☒ ☐         multiple matrices
=========  =======================  =======  =======  ============  ============  ===============  ==========  ===========================

* ^ Two checks if it's small for dense data, three checks if also for sparse. All gzipped results are small for sparse data.
* % E.g. easily supports 3D or higher arrays, unequal columns, inhomogeneous type columns...
* ~ Also tested with gzip, stats refer to non-gzipped. Gzipped is always much slower to write, a slower to read, for text formats it's at least 50% smaller.
* + Rating refers to using a semi-popular package (probably scipy), as opposed to only python and numpy.
* ++ Very easy (☒☒☒) with an unpopular and/or dedicated package, but the rating refers to only python and numpy.

You can install all dependencies using `pip install -r rements.pip`. csv and NPY were done with `numpy`_; json was done with `pyjson_tricks`_; png was done with `imgarray`_; fortran unformatted was done with `fortranfile`_; matlab was done with `scipy`_; pickle, base64 and gzipping were done with python built-ins. `pandas`_ is needed for stata (not ready yet) statai files. Seaborn is needed for plotting. You can install all dependencies using `pip install requirements.pip`

Results
---------------------------------------

Dense random matrix
=======================================

.. image:: https://raw.githubusercontent.com/mverleg/array_storage_benchmark/master/result/bm_random.png

.. image:: https://raw.githubusercontent.com/mverleg/array_storage_benchmark/master/result/bm_long.png

Sparse random matrix
=======================================

99% of values are zero, so compression ratios are very good.

.. image:: https://raw.githubusercontent.com/mverleg/array_storage_benchmark/master/result/bm_sparse.png

Real data
=======================================

Scattering probabilities for hydrogen and carbon monoxide (many doubles between 0 and 1, most close to 0). You can easily overwrite this by your own file in `testdata.csv`.

.. image:: https://raw.githubusercontent.com/mverleg/array_storage_benchmark/master/result/bm_example.png

More methods
---------------------------------------

Pull requests with other methods (serious or otherwise) are welcome! There might be some ideas in the issue tracker.


.. _`numpy`: https://docs.scipy.org/doc/numpy/reference/generated/numpy.loadtxt.html
.. _`pyjson_tricks`: https://github.com/mverleg/pyjson_tricks
.. _`imgarray`: https://github.com/mverleg/imgarray
.. _`fortranfile`: https://pypi.python.org/pypi/fortranfile/0.2.1
.. _`scipy`: https://docs.scipy.org/doc/scipy-0.18.1/reference/generated/scipy.io.loadmat.html#scipy.io.loadmat
.. _`pandas`: http://pandas.pydata.org/



