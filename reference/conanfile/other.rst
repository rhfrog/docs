Python requires
==================

It is possible to reuse python code existing in other *conanfile.py* recipes with the ``python_requires()``
functionality, doing something like:

.. code-block:: python

    from conans import python_requires
    
    base = python_requires("MyBuild/0.1@user/channel")

    class PkgTest(base.MyBase):
        ...
        def build(self):
            base.my_build(self.settings)

See this section: :ref:`Python requires: reusing python code in recipes<python_requires>`



Output and Running
==================

Output contents
---------------

Use the `self.output` to print contents to the output.

..  code-block:: python

   self.output.success("This is a good, should be green")
   self.output.info("This is a neutral, should be white")
   self.output.warn("This is a warning, should be yellow")
   self.output.error("Error, should be red")
   self.output.rewrite_line("for progress bars, issues a cr")

Check the source code. You might be able to produce different outputs with different colors.

.. _running_commands:

Running commands
----------------

.. code-block:: python

    run(self, command, output=True, cwd=None, win_bash=False, subsystem=None, msys_mingw=True,
        ignore_errors=False, run_environment=False):


``self.run()`` is a helper to run system commands and throw exceptions when errors occur,
so that command errors are do not pass unnoticed. It is just a wrapper for ``os.system()``

Optional parameters:

- **output** (Optional, Defaulted to ``True``) When True it will write in stdout.
              You can pass any stream that accepts a ``write`` method like a ``six.StringIO()``:

..  code-block:: python

    from six import StringIO  # Python 2 and 3 compatible
    mybuf = StringIO()
    self.run("mycommand", output=mybuf)
    self.output.warn(mybuf.getvalue())

- **cwd** (Optional, Defaulted to ``.`` current directory): Current directory to run the command.
- **win_bash** (Optional, Defaulted to ``False``): When True, it will run the configure/make commands inside a bash.
- **subsystem** (Optional, Defaulted to ``None`` will autodetect the subsystem). Used to escape the command according to the specified subsystem.
- **msys_mingw** (Optional, Defaulted to ``True``) If the specified subsystem is MSYS2, will start it in MinGW mode (native windows development).
- **ignore_errors** (Optional, Defaulted to ``False``). This method raises an exception if the command fails. If ``ignore_errors=True``, it
  will not raise an exception. Instead, the user can use the return code to check for errors.
- **run_environment** (Optional, Defaulted to ``False``). Applies a ``RunEnvironment``, so the environment variables PATH, LD_LIBRARY_PATH and
  DYLIB_LIBRARY_PATH are defined in the command execution adding the values of the "lib" and "bin" folders of the dependencies.
  Allows executables to be easily run using shared libraries from its dependencies.
