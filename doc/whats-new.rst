.. _whats-new:

##########
What's New
##########

.. _whats-new.0.3.2:

v0.3.2 (unreleased)
===================

Breaking Changes
----------------

- Minimum xarray version is now v0.13.  Moving forward, we will likely
  continue bumping this up so as to only support the most recent one
  or two releases of xarray at any time (:pull:`324`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.

Enhancements
------------

- Calls to ``xarray.open_mfdataset`` now use the
  ``combine='by_coords'`` option available as of xarray v0.12.2
  instead of the now-deprecated ``auto_combine`` (:pull:`324`).  By
  `Spencer Hill <https://github.com/spencerahill>`_.

- Remove obsolete checks for Python 2 vs. 3 and for obsolete warnings
  relating to bugs and/or warnings in past xarray versions no longer
  supported (:pull:`324`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.


.. _whats-new.0.3.1:

v0.3.1 (19 November 2018)
=========================

This minor release fixes the builds of our documentation, adds
continuous integration (CI) testing of those builds, introduces a
how-to guide for future releases, and otherwise slightly modifies our
docs and CI configurations.  (:pull:`313`, :pull:`314`).  These are
all backend changes --- for aospy users, there are no changes from
v0.3.0.


.. _whats-new.0.3.0:

v0.3 (15 November 2018)
=======================

This release adds a number of new features, fixes many bugs, and
improves our documentation.  It includes several deprecations, other
breaking changes, and modifications to aospy's list of required
dependencies.  We are grateful for three new contributors this time
around and are eager to grow the user- and developer-base further
moving forward!  Highlights (full changelog below these):

- Support for Python 3.7
- ``Var`` objects can now be recursively computed in terms of other
  ``Var`` objects
- Thanks to ``xarray.CFTimeIndex``, no longer require special logic/handling of
  out-of-range datetimes
- Improved handling of region longitude bounds in ``Region`` via new
  ``Longitude`` class

Breaking Changes
----------------

- Drop support for Python 2.7 and 3.4, since our core upstream dependency
  xarray is also dropping these soon (:pull:`255`, :pull:`280`).
  By `Spencer Hill <https://github.com/spencerahill>`_.
- ``aospy.Region`` no longer can be instantiated using ``lat_bounds``
  and ``lon_bounds`` keywords.  These have been replaced with the more
  explicit ``east_bound``, ``west_bound``, ``south_bound``, and
  ``north_bound`` (:pull:`266`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Deprecate ``Constant`` class and ``constants.py`` module.
  Physical constants used internally by aospy are now stored
  in ``_constants.py`` (fixes :issue:`50` via :pull:`223`).
  By `Micah Kim <https://github.com/micahkim23>`_.
- Deprecate ``Units`` class, so now the ``units`` attribute of the
  ``Var`` class is a string. (fixes :issue:`50` via :pull:`222`).
  By `Micah Kim <https://github.com/micahkim23>`_.
- Deprecate ``CalcInterface`` class.  Now, to instantiate a ``Calc``
  object, pass it directly the parameters that previously would have
  been passed to ``CalcInterface`` (fixes :issue:`249` via
  :pull:`250`).  By `Spencer Hill <https://github.com/spencerahill>`_.
- Deprecate ``utils.times.convert_scalar_to_indexable_coord``, since
  as of xarray version 0.10.3 release, the functionality is no longer
  necessary (fixes :issue:`268` via :pull:`269`.  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Deprecate ``func_input_dtype`` argument to ``Var`` (fixes
  :issue:`281` via :pull:`282`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.


Documentation
-------------

- Updates the documentation in the described ``calc_suite_specs``
  argument to ``submit_mult_calcs`` in ``automate.py`` (fixes
  :issue:`295` via :pull:`310`). By `James Doss-Gollin
  <https://github.com/jdossgollin>`_.
- Corrected link to documentation badge on repository main page
  (:pull:`213`).  `By DaCoEx <https://github.com/dacoex>`_.

Enhancements
------------

- Improve compatibility for data following IRIDL conventions or NOAA
  data formats. Specifically, several alternate names are defined in
  ``GRID_ATTRS``, while there is no longer an assumption that
  ``BOUNDS_STR`` is a coordinate of ``time_weights`` (fixes :issue:`293`
  and :issue:`299` via :pull:`309`). By `James Doss-Gollin
  <https://github.com/jdossgollin>`_.
- aospy now uses `Versioneer
  <https://github.com/warner/python-versioneer>`_ to manage its
  version strings.  By `Spencer Hill
  <https://github.com/spencerahill>`_ (:pull:`311`).
- Add support for Python 3.7. (closes :issue:`292` via :pull:`306`.
  By `Spencer Hill <https://github.com/spencerahill>`_.
- Use an ``xarray.CFTimeIndex`` for dates from non-standard calendars and
  outside the Timestamp-valid range.  This eliminates the need for the prior
  workaround, which shifted dates to within the range 1678 to 2262 prior to
  indexing (closes :issue:`98` via :pull:`273`).  By
  `Spencer Clark <https://github.com/spencerkclark>`_.
- Create ``utils.longitude`` module and ``Longitude`` class for
  representing and comparing longitudes.  Used internally by
  ``aospy.Region`` to construct masks, but could also be useful for
  users outside the standard aospy workflow (:pull:`266`).  By
  `Spencer Hill <https://github.com/spencerahill>`_.
- Add support for ``Region`` methods ``mask_var``, ``ts``, ``av``, and
  ``std`` for data that doesn't conform to aospy naming conventions,
  making these methods now useful in more interactive contexts in
  addition to within the standard main script-based work flow
  (:pull:`266`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Raise an exception with an informative message if
  ``submit_mult_calcs`` (and thus the main script) generates zero
  calculations, which can happen if one of the parameters is
  accidentally set to an empty list (closes :issue:`253` via
  :pull:`254`).  By `Spencer Hill <https://github.com/spencerahill>`_.
- Suppress warnings from xarray when loading data whose dates extend
  outside the range supported by the numpy.datetime64 datatype.  aospy
  has its own logic to deal with these cases (closes :issue:`221` via
  :pull:`239`).  By `Spencer Hill <https://github.com/spencerahill>`_.
- Add units and description from ``Var`` objects to output netcdf
  files (closes :issue:`201` via :pull:`232`). By `Micah Kim
  <https://github.com/micahkim23>`_.
- Remove potentially confusing attributes from example netcdf files.
  (closes :issue:`214` via :pull:`216`). By `Micah Kim
  <https://github.com/micahkim23>`_.
- Cleanup logic for Dataset drop on dimensions with and without
  coords. Use Dataset isel instead. (closes :issue:`142` via
  :pull:`241`). By `Micah Kim <https://github.com/micahkim23>`_.
- Expose ``data_vars`` and ``coords`` options to ``xr.open_mfdataset``
  in DataLoaders.  These options control how variables and coordinates are
  concatenated when loaded in from multiple files; by default ``aospy``
  uses ``data_vars='minimal'`` and ``coords='minimal'``, but there could
  be use cases where other options are desired.  See `the xarray documentation
  <http://xarray.pydata.org/en/stable/generated/xarray.open_mfdataset.html>`_
  for more information (closes :issue:`236` via :pull:`240`).  By `Spencer
  Clark <https://github.com/spencerkclark>`_.
- Allow for variables to be functions of other computed variables (closes
  :issue:`3` via :pull:`263`).  By `Spencer
  Clark <https://github.com/spencerkclark>`_.
- Add a ``grid_attrs`` argument to the :py:class:`~aospy.Model` constructor to
  allow the specification of custom alternative names for grid attributes like
  time, latitude, or longitude (closes :issue:`182` via :pull:`297`). By
  `Spencer Clark <https://github.com/spencerkclark>`_.

Bug Fixes
---------

- Use the new ``Longitude`` class to support any longitude numbering
  convention (e.g. -180 to 180, 0 to 360, or any other) for both
  defining ``Region`` objects and for input data to be masked.  Fixes
  bug wherein a region could be silently partially clipped off when
  masking input data with longitudes of a different numbering
  convention.  Fixes :issue:`229` via :pull:`266`.  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Cast input DataArrays with datatype ``np.float32`` to ``np.float64``
  as a workaround for incorrectly computed means on float32 arrays in
  bottleneck (see `pydata/xarray#1346
  <https://github.com/pydata/xarray/issues/1346>`_).  If one would
  like to disable this behavior (i.e. restore the original behavior
  before this fix), one can set the ``upcast_float32`` keyword
  argument in their DataLoaders to ``False``.  Fixes :issue:`217` via
  :pull:`218`.  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Switch from using ``scipy`` to ``netcdf4`` as the engine when
  writing to netCDF files to avoid bugs when using ``libnetcdf``
  version 4.5.0 (:pull:`235`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- ``CalcSuite`` (and thus ``submit_mult_calc``) now skips calculations
  that involve time reductions of non-time-defined variables. ``Calc``
  now raises a ValueError when instantiated with a non-time-defined
  variable but has one or more time-defined reductions. (closes
  :issue:`202` via :pull:`242`). By `Micah Kim
  <https://github.com/micahkim23>`_.


Testing
-------

- Create Travis CI environment that tests against the xarray
  development branch. (closes :issue:`224` via :pull: `226`).
  By `Micah Kim <https://github.com/micahkim23>`_.
- Use ``nbconvert`` and ``nbformat`` rather than ``runipy`` to test
  the tutorial Jupyter notebook, as ``runipy`` `is deprecated
  <https://github.com/paulgb/runipy/blob/master/README.rst>`_
  (:pull:`239`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Add flake8 to Travis CI environment to check that new code
  adheres to pep8 style. Add verbose flag to pytest test suite.
  (closes :issue:`234` via :pull:`237`). By `Micah Kim
  <https://github.com/micahkim23>`_.

Dependencies
------------

- ``aospy`` now requires a minimum version of ``distributed`` of
  1.17.1 (fixes :issue:`210` via :pull:`211`).
- ``aospy`` now requires a minimum version of ``xarray`` of 0.10.6.
  See discussion in :issue:`199`, :pull:`240`, :issue:`268`,
  :pull:`269`, :pull:`273`, and :pull:`275` for more details.


.. _whats-new.0.2:

v0.2 (26 September 2017)
========================

This release includes some new features plus several bugfixes.  The
bugfixes include some that previously made using aospy on
pressure-interpolated data very problematic.  We have also improved
support for reading in data from the WRF and CAM atmospheric models.

As of this release, aospy has at least 2(!) confirmed regular users
that aren't the original aospy developers, bringing the worldwide
total of users up to at least 4.  The first user-generated Github
Issues have now also been created.  We're a real thing!

Enhancements
------------

- Use ``dask.bag`` coupled with ``dask.distributed`` rather than
  ``multiprocess`` to parallelize computations (closes :issue:`169`
  via :pull:`172`).  This enables the optional use of an external
  ``distributed.Client`` to leverage computational resources across
  multiple nodes of a cluster. By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Improve support for WRF and NCAR CAM model data by adding the
  internal names they use for grid attributes to aospy's lists of
  potential names to search for.  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Allow a user to specify a custom preprocessing function in all
  DataLoaders to prepare data for processing with aospy.  This could
  be used, for example, to add a CF-compliant units attribute to the
  time coordinate if it is not present in a set of files.  Addresses
  :issue:`177` via :pull:`180`.  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Remove ``dask.async`` import in ``model.py``; no longer needed, and
  also prevents warning message from dask regarding location of
  ``get_sync`` function  (:pull:`195`).  By
  `Spencer Hill <https://github.com/spencerahill>`_.


Dependencies
------------

- ``multiprocess`` is no longer required for submitting ``aospy``
  calculations in parallel (see discussion in :issue:`169` and pull
  request :pull:`172`).
- ``aospy`` now requires an installation of ``dask`` with version
  greater than or equal to 0.14 (see discussion in pull request
  :pull:`172`).

Bug Fixes
---------

- Remove faulty logic for calculations with data coming from multiple
  runs.  Eventually this feature will be properly implemented (fixes
  :issue:`117` via :pull:`178`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Only run tests that require optional dependencies if those
  dependencies are actually installed (fixes :issue:`167` via
  :pull:`176`).  By `Spencer Hill <https://github.com/spencerahill>`_.
- Remove obsolete ``operator.py`` module (fixes :issue:`174` via
  :pull:`175`).  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Fix workaround for dates with years less than 1678 to support units
  attributes with a reference date years not equal to 0001 (fixes
  :issue:`188` via :pull:`189`).  By
  `Spencer Clark <https://github.com/spencerkclark>`_.
- Fix bug which would prevent users from analyzing a subset within the
  Timestamp-valid range from a dataset which
  included data from outside the Timestamp-valid range (fixed in
  :pull:`189`). By
  `Spencer Clark <https://github.com/spencerkclark>`_.
- Toggle the ``mask_and_scale`` option to ``True`` when reading in
  netCDF files to enable missing values encoded as floats to be
  converted to NaN's (fixes :issue:`190` via :pull:`192`).  By
  `Spencer Clark <https://github.com/spencerkclark>`_.
- Force regional calculations to mask gridcell weights where the
  loaded datapoints were invalid instead of just masking points
  outside the desired region (fixes :issue:`190` via :pull:`192`).  By
  `Spencer Clark <https://github.com/spencerkclark>`_.
- Retain original input data's mask during gridpoint-by-gridpoint
  temporal averages (fixes :issue:`193` via :pull:`196`).  By `Spencer
  Hill <https://github.com/spencerahill>`_.
- Always write output to a tar file in serial to prevent empty header file
  errors (fixes :issue:`75` via :pull:`197`).  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Allow ``aospy`` to use grid attributes that are only defined in ``Run``
  objects. Previously if a grid attribute were defined only in a ``Run``
  object and not also in the Run's corresponding ``Model``, an error would
  be raised (fixes :issue:`187` via :pull:`199`).  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- When input data for a calculation has a time bounds array, overwrite
  its time array with the average of the start and end times for each
  timestep.  Prevents bug wherein time arrays equal to either the
  start or end bounds get mistakenly grouped into the wrong time
  interval, i.e. the wrong month or year (fixes :issue `185` via
  :pull:`200`).  By `Spencer Hill <https://github.com/spencerahill>`_.


.. _whats-new.0.1.2:

v0.1.2 (30 March 2017)
======================

This release improves the process of submitting multiple calculations
for automatic execution.  The user interface, documentation, internal
logic, and packaging all received upgrades and/or bugfixes.

We also now have a `mailing list`_.  Join it to follow and/or post
your own usage questions, bug reports, suggestions, etc.

.. _mailing list: https://groups.google.com/d/forum/aospy

Enhancements
------------

- Include an example library of aospy objects that works
  out-of-the-box with the provided example main script (:pull:`155`).
  By `Spencer Clark <https://github.com/spencerkclark>`_ and `Spencer
  Hill <https://github.com/spencerahill>`_.
- Improve :ref:`examples` page of the documentation by using this new
  example object library (:pull:`164`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Improve readability/usability of the included example script
  ``aospy_main.py`` for submitting aospy calculations by moving all
  internal logic into new ``automate.py`` module (:pull:`155`).  By
  `Spencer Clark <https://github.com/spencerkclark>`_ and `Spencer
  Hill <https://github.com/spencerahill>`_.
- Enable user to specify whether or not to write output to .tar files
  (in addition to the standard output).  Also document an error that
  occurs when writing output to .tar files for sufficiently old
  versions of tar (including the version that ships standard on
  MacOS), and print a warning when errors are caught during the 'tar'
  call (:pull:`160`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.

Bug fixes
---------

- Update packaging specifications such that the example main script
  and tutorial notebook actually ship with aospy as intended (fixes
  :issue:`149` via :pull:`161`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Use the 'scipy' engine for the `xarray.DataArray.to_netcdf`_
  call when writing aospy calculation outputs to disk to prevent a bug
  when trying to re-write to an existing netCDF file (fixes
  :issue:`157` via :pull:`160`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.

.. _xarray.DataArray.to_netcdf : http://xarray.pydata.org/en/stable/generated/xarray.DataArray.to_netcdf.html


.. _whats-new.0.1.1:

v0.1.1 (2 March 2017)
=====================

This release includes fixes for a number of bugs mistakenly introduced
in the refactoring of the variable loading step of ``calc.py``
(:pull:`90`), as well as support for xarray version 0.9.1.

Enhancements
------------

- Support for xarray version 0.9.1 and require it or a later xarray
  version.  By `Spencer Clark <https://github.com/spencerkclark>`_ and
  `Spencer Hill <https://github.com/spencerahill>`_.
- Better support for variable names relating to "bounds" dimension of
  input data files.  "bnds", "bounds", and "nv" now all supported
  (:pull:`140`).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- When coercing dims of input data to aospy's internal names, for
  scalars change only the name; for non-scalars change the name, force
  them to have a coord, and copy over their attrs (:pull:`140`).  By
  `Spencer Hill <https://github.com/spencerahill>`_.

Bug fixes
---------

- Fix bug involving loading data that has dims that lack coords (which
  is possible as of xarray v0.9.0).  By `Spencer Hill
  <https://github.com/spencerahill>`_.
- Fix an instance where the name for pressure half levels was
  mistakenly replaced with the name for the pressure full levels
  (:pull:`126`).  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Prevent workaround for dates outside the ``pd.Timestamp`` valid
  range from being applied to dates within the ``pd.Timestamp`` valid
  range (:pull:`128`).  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Ensure that all DataArrays associated with :py:class:`aospy.Var`
  objects have a time weights coordinate with CF-compliant time units.
  This allows them to be cast as the type ``np.timedelta64``, and be
  safely converted to have units of days before taking time-weighted
  averages (:pull:`128`).  By `Spencer Clark
  <https://github.com/spencerkclark>`_.
- Fix a bug where the time weights were not subset in time prior to
  taking a time weighted average; this caused computed seasonal
  averages to be too small.  To prevent this from failing silently
  again, we now raise a ``ValueError`` if the time coordinate of the
  time weights is not identical to the time coordinate of the array
  associated with the :py:class:`aospy.Var` (:pull:`128`).  By
  `Spencer Clark <https://github.com/spencerkclark>`_.
- Enable calculations to be completed using data saved as a single
  time-slice on disk (fixes :issue:`132` through :pull:`135`).  By
  `Spencer Clark <https://github.com/spencerkclark>`_.
- Fix bug where workaround for dates outside the ``pd.Timestamp``
  valid range caused a mismatch between the data loaded and the data
  requested (fixes :issue:`138` through :pull:`139`). By `Spencer
  Clark <https://github.com/spencerkclark>`_.


.. _whats-new.0.1:

v0.1 (24 January 2017)
======================

- Initial release!
- Contributors:

  - `Spencer Hill <https://github.com/spencerahill>`_
  - `Spencer Clark <https://github.com/spencerkclark>`_
