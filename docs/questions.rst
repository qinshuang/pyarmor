.. _questions:

When Things Go Wrong
====================

Could not load `_pytransform`
-----------------------------

Generally, the dynamic library `_pytransform` is in the same path of
obfuscated scripts. It may be:

* `_pytransform.so` in Linux
* `_pytransform.dll` in Windows
* `_pytransform.dylib` in MacOS

First check whether the file exists. If it exists:

* Check the permissions of dynamic library

    If there is no execute permissions in Windows, it will complain:
    `[Error 5] Access is denied`

* Check whether `ctypes` could load `_pytransform`::

    from pytransform import _load_library
    m = _load_library(path='/path/to/dist')

* Try to set the runtime path in the :ref:`Bootstrap Code` of entry
  script::

    from pytransform import pyarmor_runtime
    pyarmor_runtime('/path/to/dist')

Still doesn't work, report an issue_

The `license.lic` generated doesn't work
----------------------------------------

The key is that the capsule used to obfuscate scripts must be same as
the capsule used to generate licenses.

If obfuscate scripts by command `pyarmor obfuscate`, :ref:`Global
Capsule` is used implicitly. If obfuscate scripts by command `pyarmor
build`, the project capsule in the project path is used.

When generating new licenses for obfuscated scripts, if run command
`pyarmor licenses` in project path, the project capsule is used
implicitly, otherwise :ref:`Global Capsule`.

The :ref:`Global Capsule` will be changed if the trial license file of
|PyArmor| is replaced with normal one, or it's deleted occasionally
(which will be generated implicitly as running command `pyarmor
obfuscate` next time).

The project capsule is overwrited when running command `pyarmor init`
in the project path created before.

In any cases, generating new license file with the different capsule
will not work for the obfuscated scripts before. If the old capsule is
gone, one solution is to obfuscate these scripts by the new capsule
again.

Two types of `license.lic`
--------------------------

In pyarmor, there are 2 types of `license.lic`

* `license.lic` of |PyArmor|, which locates in the source path of
  |PyArmor|. It's required to run `pyarmor`

* `license.lic` of Obfuscated Scripts, which is generated as
  obfuscating scripts or generating new licenses. It's required to run
  obfuscated scripts.

Each project has its own capsule `.pyarmor_capsule.zip` in project
path. This capsule is generated when run command `pyarmor init` to
create a project. And `license.lic` of |PyArmor| will be as an input
file to make this capsule.

When runing command `pyarmor build` or `pyarmor licenses`, it will
generate a `license.lic` in project output path for obfuscated
scripts. Here the project capsule `.pyarmor_capsule.zip` will be as
input file to generate this `license.lic` of Obfuscated Scripts.

So the relation between 2 `license.lic` is::

    license.lic of PyArmor --> .pyarmor_capsule.zip --> license.lic of Obfuscated Scripts

If the scripts are obfuscated by command `pyarmor obfuscate` other
than by project, :ref:`Global Capsule` is used implicitly.


.. include:: _common_definitions.txt
