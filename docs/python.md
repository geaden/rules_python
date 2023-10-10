<!-- Generated with Stardoc: http://skydoc.bazel.build -->

Core rules for building Python projects.

<a id="current_py_toolchain"></a>

## current_py_toolchain

<pre>
current_py_toolchain(<a href="#current_py_toolchain-name">name</a>)
</pre>

This rule exists so that the current python toolchain can be used in the `toolchains` attribute of
other rules, such as genrule. It allows exposing a python toolchain after toolchain resolution has
happened, to a rule which expects a concrete implementation of a toolchain, rather than a
toolchain_type which could be resolved to that toolchain.

**ATTRIBUTES**


| Name  | Description | Type | Mandatory | Default |
| :------------- | :------------- | :------------- | :------------- | :------------- |
| <a id="current_py_toolchain-name"></a>name |  A unique name for this target.   | <a href="https://bazel.build/concepts/labels#target-names">Name</a> | required |  |


<a id="py_import"></a>

## py_import

<pre>
py_import(<a href="#py_import-name">name</a>, <a href="#py_import-deps">deps</a>, <a href="#py_import-srcs">srcs</a>)
</pre>

This rule allows the use of Python packages as dependencies.

It imports the given `.egg` file(s), which might be checked in source files,
fetched externally as with `http_file`, or produced as outputs of other rules.

It may be used like a `py_library`, in the `deps` of other Python rules.

This is similar to [java_import](https://docs.bazel.build/versions/master/be/java.html#java_import).

**ATTRIBUTES**


| Name  | Description | Type | Mandatory | Default |
| :------------- | :------------- | :------------- | :------------- | :------------- |
| <a id="py_import-name"></a>name |  A unique name for this target.   | <a href="https://bazel.build/concepts/labels#target-names">Name</a> | required |  |
| <a id="py_import-deps"></a>deps |  The list of other libraries to be linked in to the binary target.   | <a href="https://bazel.build/concepts/labels">List of labels</a> | optional |  `[]`  |
| <a id="py_import-srcs"></a>srcs |  The list of Python package files provided to Python targets that depend on this target. Note that currently only the .egg format is accepted. For .whl files, try the whl_library rule. We accept contributions to extend py_import to handle .whl.   | <a href="https://bazel.build/concepts/labels">List of labels</a> | optional |  `[]`  |


<a id="PyInfo"></a>

## PyInfo

<pre>
PyInfo(<a href="#PyInfo-transitive_sources">transitive_sources</a>, <a href="#PyInfo-uses_shared_libraries">uses_shared_libraries</a>, <a href="#PyInfo-imports">imports</a>, <a href="#PyInfo-has_py2_only_sources">has_py2_only_sources</a>,
       <a href="#PyInfo-has_py3_only_sources">has_py3_only_sources</a>)
</pre>

Encapsulates information provided by the Python rules.

**FIELDS**


| Name  | Description |
| :------------- | :------------- |
| <a id="PyInfo-transitive_sources"></a>transitive_sources |  A (`postorder`-compatible) depset of `.py` files appearing in the target's `srcs` and the `srcs` of the target's transitive `deps`.    |
| <a id="PyInfo-uses_shared_libraries"></a>uses_shared_libraries |  Whether any of this target's transitive `deps` has a shared library file (such as a `.so` file).<br><br>This field is currently unused in Bazel and may go away in the future.    |
| <a id="PyInfo-imports"></a>imports |  A depset of import path strings to be added to the `PYTHONPATH` of executable Python targets. These are accumulated from the transitive `deps`. The order of the depset is not guaranteed and may be changed in the future. It is recommended to use `default` order (the default).    |
| <a id="PyInfo-has_py2_only_sources"></a>has_py2_only_sources |  Whether any of this target's transitive sources requires a Python 2 runtime.    |
| <a id="PyInfo-has_py3_only_sources"></a>has_py3_only_sources |  Whether any of this target's transitive sources requires a Python 3 runtime.    |


<a id="PyRuntimeInfo"></a>

## PyRuntimeInfo

<pre>
PyRuntimeInfo(<a href="#PyRuntimeInfo-interpreter_path">interpreter_path</a>, <a href="#PyRuntimeInfo-interpreter">interpreter</a>, <a href="#PyRuntimeInfo-files">files</a>, <a href="#PyRuntimeInfo-coverage_tool">coverage_tool</a>, <a href="#PyRuntimeInfo-coverage_files">coverage_files</a>, <a href="#PyRuntimeInfo-python_version">python_version</a>,
              <a href="#PyRuntimeInfo-stub_shebang">stub_shebang</a>, <a href="#PyRuntimeInfo-bootstrap_template">bootstrap_template</a>)
</pre>

Contains information about a Python runtime, as returned by the `py_runtime`
rule.

A Python runtime describes either a *platform runtime* or an *in-build runtime*.
A platform runtime accesses a system-installed interpreter at a known path,
whereas an in-build runtime points to a `File` that acts as the interpreter. In
both cases, an "interpreter" is really any executable binary or wrapper script
that is capable of running a Python script passed on the command line, following
the same conventions as the standard CPython interpreter.

**FIELDS**


| Name  | Description |
| :------------- | :------------- |
| <a id="PyRuntimeInfo-interpreter_path"></a>interpreter_path |  If this is a platform runtime, this field is the absolute filesystem path to the interpreter on the target platform. Otherwise, this is `None`.    |
| <a id="PyRuntimeInfo-interpreter"></a>interpreter |  If this is an in-build runtime, this field is a `File` representing the interpreter. Otherwise, this is `None`. Note that an in-build runtime can use either a prebuilt, checked-in interpreter or an interpreter built from source.    |
| <a id="PyRuntimeInfo-files"></a>files |  If this is an in-build runtime, this field is a `depset` of `File`sthat need to be added to the runfiles of an executable target that uses this runtime (in particular, files needed by `interpreter`). The value of `interpreter` need not be included in this field. If this is a platform runtime then this field is `None`.    |
| <a id="PyRuntimeInfo-coverage_tool"></a>coverage_tool |  If set, this field is a `File` representing tool used for collecting code coverage information from python tests. Otherwise, this is `None`.    |
| <a id="PyRuntimeInfo-coverage_files"></a>coverage_files |  The files required at runtime for using `coverage_tool`. Will be `None` if no `coverage_tool` was provided.    |
| <a id="PyRuntimeInfo-python_version"></a>python_version |  Indicates whether this runtime uses Python major version 2 or 3. Valid values are (only) `"PY2"` and `"PY3"`.    |
| <a id="PyRuntimeInfo-stub_shebang"></a>stub_shebang |  "Shebang" expression prepended to the bootstrapping Python stub script used when executing `py_binary` targets.  Does not apply to Windows.    |
| <a id="PyRuntimeInfo-bootstrap_template"></a>bootstrap_template |  See py_runtime_rule.bzl%py_runtime.bootstrap_template for docs.    |


<a id="py_binary"></a>

## py_binary

<pre>
py_binary(<a href="#py_binary-attrs">attrs</a>)
</pre>

See the Bazel core [py_binary](https://docs.bazel.build/versions/master/be/python.html#py_binary) documentation.

**PARAMETERS**


| Name  | Description | Default Value |
| :------------- | :------------- | :------------- |
| <a id="py_binary-attrs"></a>attrs |  Rule attributes   |  none |


<a id="py_library"></a>

## py_library

<pre>
py_library(<a href="#py_library-attrs">attrs</a>)
</pre>

See the Bazel core [py_library](https://docs.bazel.build/versions/master/be/python.html#py_library) documentation.

**PARAMETERS**


| Name  | Description | Default Value |
| :------------- | :------------- | :------------- |
| <a id="py_library-attrs"></a>attrs |  Rule attributes   |  none |


<a id="py_runtime"></a>

## py_runtime

<pre>
py_runtime(<a href="#py_runtime-attrs">attrs</a>)
</pre>

See the Bazel core [py_runtime](https://docs.bazel.build/versions/master/be/python.html#py_runtime) documentation.

**PARAMETERS**


| Name  | Description | Default Value |
| :------------- | :------------- | :------------- |
| <a id="py_runtime-attrs"></a>attrs |  Rule attributes   |  none |


<a id="py_runtime_pair"></a>

## py_runtime_pair

<pre>
py_runtime_pair(<a href="#py_runtime_pair-name">name</a>, <a href="#py_runtime_pair-py2_runtime">py2_runtime</a>, <a href="#py_runtime_pair-py3_runtime">py3_runtime</a>, <a href="#py_runtime_pair-attrs">attrs</a>)
</pre>

A toolchain rule for Python.

This used to wrap up to two Python runtimes, one for Python 2 and one for Python 3.
However, Python 2 is no longer supported, so it now only wraps a single Python 3
runtime.

Usually the wrapped runtimes are declared using the `py_runtime` rule, but any
rule returning a `PyRuntimeInfo` provider may be used.

This rule returns a `platform_common.ToolchainInfo` provider with the following
schema:

```python
platform_common.ToolchainInfo(
    py2_runtime = None,
    py3_runtime = <PyRuntimeInfo or None>,
)
```

Example usage:

```python
# In your BUILD file...

load("@rules_python//python:defs.bzl", "py_runtime_pair")

py_runtime(
    name = "my_py3_runtime",
    interpreter_path = "/system/python3",
    python_version = "PY3",
)

py_runtime_pair(
    name = "my_py_runtime_pair",
    py3_runtime = ":my_py3_runtime",
)

toolchain(
    name = "my_toolchain",
    target_compatible_with = <...>,
    toolchain = ":my_py_runtime_pair",
    toolchain_type = "@rules_python//python:toolchain_type",
)
```

```python
# In your WORKSPACE...

register_toolchains("//my_pkg:my_toolchain")
```


**PARAMETERS**


| Name  | Description | Default Value |
| :------------- | :------------- | :------------- |
| <a id="py_runtime_pair-name"></a>name |  str, the name of the target   |  none |
| <a id="py_runtime_pair-py2_runtime"></a>py2_runtime |  optional Label; must be unset or None; an error is raised otherwise.   |  `None` |
| <a id="py_runtime_pair-py3_runtime"></a>py3_runtime |  Label; a target with `PyRuntimeInfo` for Python 3.   |  `None` |
| <a id="py_runtime_pair-attrs"></a>attrs |  Extra attrs passed onto the native rule   |  none |


<a id="py_test"></a>

## py_test

<pre>
py_test(<a href="#py_test-attrs">attrs</a>)
</pre>

See the Bazel core [py_test](https://docs.bazel.build/versions/master/be/python.html#py_test) documentation.

**PARAMETERS**


| Name  | Description | Default Value |
| :------------- | :------------- | :------------- |
| <a id="py_test-attrs"></a>attrs |  Rule attributes   |  none |


<a id="find_requirements"></a>

## find_requirements

<pre>
find_requirements(<a href="#find_requirements-name">name</a>)
</pre>

The aspect definition. Can be invoked on the command line as

bazel build //pkg:my_py_binary_target         --aspects=@rules_python//python:defs.bzl%find_requirements         --output_groups=pyversioninfo

**ASPECT ATTRIBUTES**


| Name | Type |
| :------------- | :------------- |
| deps| String |


**ATTRIBUTES**


| Name  | Description | Type | Mandatory | Default |
| :------------- | :------------- | :------------- | :------------- | :------------- |
| <a id="find_requirements-name"></a>name |  A unique name for this target.   | <a href="https://bazel.build/concepts/labels#target-names">Name</a> | required |  |


