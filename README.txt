======================================================
CMA-ES Covariance Matrix Adaptation Evolution Strategy
======================================================

a stochastic numerical optimization algorithm for difficult (non-convex,
ill-conditioned, multi-modal, rugged, noisy) optimization problems in
continuous search spaces, implemented in Python.

Typical domain of application are bound-constrained or unconstrained
objective functions with:

* search space dimension between 5 and 100,
* no gradients available,
* no less than, say, 100 times dimension function evaluations needed to
  get satisfactory solutions,
* non-separable, ill-conditioned, or rugged/multi-modal landscapes.

The CMA-ES is quite reliable, however for small budgets (fewer than 100
times dimension function evaluations) or in very small dimensions
better methods are available.

Installation
------------
There are several ways of installation:

* In the terminal command line simply type::

      pip install cma

  or, alternatively::

      easy_install cma

  The package will be downloaded and installed automatically. To
  **upgrade** an existing installation, '``cma``' must be replaced by 
  '``-U cma``' in both cases. If you never heard of ``pip``, 
  `see here`__.


  __ http://www.pip-installer.org

* Download and unpack the ``cma-...tar.gz`` file and type::

      python setup.py install

  in the ``cma-...`` folder (under Windows just
  "``setup.py install``").

* Under Windows one may also download the MS Windows installer.


Installation **might require root privileges** and therefore commands 
might need to be prepended with ``sudo``.

The file ``cma.py`` from the ``tar`` archive can also be used without
any installation (just ``import`` needs to find it).

Usage Example
-------------
In a Python shell::

    >>> import cma
    >>> es = cma.CMAEvolutionStrategy(8 * [0], 0.5)
    (5_w,10)-aCMA-ES (mu_w=3.2,w_1=45%) in dimension 8 (seed=468976, Tue May  6 19:14:06 2014)
    >>> help(es)  # the same as help(cma.CMAEvolutionStrategy)
        <output omitted>
    >>> es.optimize(cma.fcts.rosen)
    Iterat #Fevals   function value    axis ratio  sigma  minstd maxstd min:sec
        1      10 1.042661803766204e+02 1.0e+00 4.50e-01  4e-01  5e-01 0:0.0
        2      20 7.322331708590002e+01 1.2e+00 3.89e-01  4e-01  4e-01 0:0.0
        3      30 6.048150359372417e+01 1.2e+00 3.47e-01  3e-01  3e-01 0:0.0
      100    1000 3.165939452385367e+00 1.1e+01 7.08e-02  2e-02  7e-02 0:0.2
      200    2000 4.157333035296804e-01 1.9e+01 8.10e-02  9e-03  5e-02 0:0.4
      300    3000 2.413696640005903e-04 4.3e+01 9.57e-03  3e-04  7e-03 0:0.5
      400    4000 1.271582136805314e-11 7.6e+01 9.70e-06  8e-08  3e-06 0:0.7
      439    4390 1.062554035878040e-14 9.4e+01 5.31e-07  3e-09  8e-08 0:0.8
    >>> es.result_pretty()  # pretty print result
    termination on tolfun=1e-11
    final/bestever f-value = 3.729752e-15 3.729752e-15
    mean solution: [ 1.          1.          1.          1.          0.99999999  0.99999998
      0.99999995  0.99999991]
    std deviation: [  2.84303359e-09   2.74700402e-09   3.28154576e-09   5.92961588e-09
       1.07700123e-08   2.12590385e-08   4.09374304e-08   8.16649754e-08]

optimizes the 8-dimensional Rosenbrock function with initial solution all
zeros and initial ``sigma = 0.5``.

Pretty much the same can be achieved a little less "elaborate" with::

    >>> import cma
    >>> res = cma.fmin(cma.fcts.rosen, 8 * [0], 0.5)
        <output omitted>

And a little more elaborate exposing the **ask-and-tell interface**::

    >>> import cma
    >>> es = cma.CMAEvolutionStrategy(12 * [0], 0.5)
    >>> while not es.stop():
    ...     solutions = es.ask()
    ...     es.tell(solutions, [cma.fcts.rosen(x) for x in solutions])
    ...     es.logger.add()  # write data to disc to be plotted
    ...     es.disp()
        <output omitted>
    >>> es.result_pretty()
        <output omitted>
    >>> cma.plot()  # shortcut for es.logger.plot()

.. figure:: https://www.lri.fr/~hansen/rosen12.png
    :alt: CMA-ES on Rosenbrock function in dimension 8
    :target: https://www.lri.fr/~hansen/cmaes_inmatlab.html#example
    :align: center 
   
    A single run on the 12-dimensional Rosenbrock function. 


The ``CMAOptions`` class manages options for ``CMAEvolutionStrategy``,
e.g. verbosity options can be found like::

    >>> import cma
    >>> from cma import pprint as pp  # several imports per line are considered as bad style
    >>> pp(cma.CMAOptions('erb'))
    {'verb_log': '1  #v verbosity: write data to files every verb_log iteration, writing can be time critical on fast to evaluate functions'
     'verbose': '1  #v verbosity e.v. of initial/final message, -1 is very quiet, not yet implemented'
     'verb_plot': '0  #v in fmin(): plot() is called every verb_plot iteration'
     'verb_disp': '100  #v verbosity: display console output every verb_disp iteration'
     'verb_filenameprefix': 'outcmaes  # output filenames prefix'
     'verb_append': '0  # initial evaluation counter, if append, do not overwrite output files'
     'verb_time': 'True  #v output timings on console'}

Options are passed like::

    >>> import cma
    >>> es = cma.CMAEvolutionStrategy(8 * [0], 0.5,
                                      {'verb_disp': 1}) # display each iteration


Documentations
--------------
Read the `full package documentation here`__.

__ https://www.lri.fr/~hansen/html-pythoncma

See also

* `code home page`_ with practical hints
* `CMA-ES on Wikipedia`_

.. _`code home page`: https://www.lri.fr/~hansen/cmaes_inmatlab.html
.. _`CMA-ES on Wikipedia`: http://en.wikipedia.org/wiki/CMA-ES

Dependencies
------------

* required: ``numpy`` -- array processing for numbers, strings, records, and objects
* optional (highly recommended): ``matplotlib`` -- Python plotting package (includes ``pylab``)

Use ``pip install numpy`` etc. for installation. For a Python implementation of CMA-ES with lesser dependencies see here__.

__ https://www.lri.fr/~hansen/cmaes_inmatlab.html#python

Author: Nikolaus Hansen, 2008-2014.

License: BSD
