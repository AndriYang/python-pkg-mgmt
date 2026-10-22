Automated update to requirements files following changes to `requirements.in` in commit 05555c1d9840267c617ae5f1efefd512160cd7af.

**Files updated:**
- `detailed_requirements.txt` — full pinned dependency graph (pip-compile output)
- `requirements.txt` — flat pinned list (comments and extras stripped)
- `reverse_dependency.txt` — reverse dependency tree (pipdeptree output)

Review the diff and the three audit sections below, then merge.

---

## ✅ Orphan candidates

Transitive packages in `requirements.in` that pip-compile can no longer trace to any upstream package — candidates for removal to slim the Docker image. Tag with `# runtime-override: <reason>` to suppress if needed at runtime.

<details>
<summary>Details</summary>

```
No orphan candidates found.
```

</details>

---

## ⚠️ New / untracked packages

Packages that pip-compile resolved but that are not listed anywhere in `requirements.in`. Items marked `*** NEW THIS RUN ***` appeared for the first time in this compile. Add them to the appropriate Transitive Libraries section.

<details>
<summary>Details</summary>

```
31 package(s) resolved by pip-compile are not listed in requirements.in:
  - arro3-core
  - azure-core
  - azure-storage-blob
  - certifi
  - cffi
  - charset-normalizer
  - click
  - cryptography
  - defusedxml
  - deprecated
  - et-xmlfile
  - idna
  - isodate
  - joblib
  - msal
  - msal-extensions
  - mypy-extensions
  - opencv-python-headless
  - pillow
  - playa-pdf
  - pycparser
  - pyjwt
  - pypdfium2
  - python-dateutil
  - regex
  - six
  - tabulate
  - tqdm
  - typing-extensions
  - urllib3
  - wrapt

Add them to the appropriate 'Transitive Libraries' section of requirements.in to make the dependency explicit.
```

</details>

---

## ✅ Kedro smoke tests passed

A minimal kedro pipeline and kedro-azureml import test ran against the compiled requirements to catch missing runtime dependencies.

<details>
<summary>Test output</summary>

```
============================= test session starts ==============================
platform linux -- Python 3.14.8, pytest-9.1.1, pluggy-1.6.0 -- /usr/local/py-utils/venvs/pytest/bin/python
cachedir: .pytest_cache
rootdir: /__w/python-pkg-mgmt/python-pkg-mgmt
collecting ... collected 3 items

tests/smoke/test_kedro_pipeline.py::test_kedro_core_imports FAILED       [ 33%]
tests/smoke/test_kedro_pipeline.py::test_kedro_azureml_imports FAILED    [ 66%]
tests/smoke/test_kedro_pipeline.py::test_pipeline_runs_locally FAILED    [100%]

=================================== FAILURES ===================================
___________________________ test_kedro_core_imports ____________________________

    def test_kedro_core_imports():
        """Core kedro modules must be importable."""
>       from kedro.io import DataCatalog, MemoryDataset  # noqa: F401
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
E       ModuleNotFoundError: No module named 'kedro'

tests/smoke/test_kedro_pipeline.py:12: ModuleNotFoundError
__________________________ test_kedro_azureml_imports __________________________

    def test_kedro_azureml_imports():
        """kedro-azureml and its runtime dependencies must be importable.
    
        cachetools is a known runtime dependency of kedro-azureml that is not
        always surfaced by pip-compile — if it goes missing, this test will catch it.
        """
>       import kedro_azureml  # noqa: F401
        ^^^^^^^^^^^^^^^^^^^^
E       ModuleNotFoundError: No module named 'kedro_azureml'

tests/smoke/test_kedro_pipeline.py:23: ModuleNotFoundError
__________________________ test_pipeline_runs_locally __________________________

    def test_pipeline_runs_locally():
        """A minimal two-node pipeline must execute end-to-end with a local runner."""
>       from kedro.io import DataCatalog, MemoryDataset
E       ModuleNotFoundError: No module named 'kedro'

tests/smoke/test_kedro_pipeline.py:31: ModuleNotFoundError
=========================== short test summary info ============================
FAILED tests/smoke/test_kedro_pipeline.py::test_kedro_core_imports - ModuleNotFoundError: No module named 'kedro'
FAILED tests/smoke/test_kedro_pipeline.py::test_kedro_azureml_imports - ModuleNotFoundError: No module named 'kedro_azureml'
FAILED tests/smoke/test_kedro_pipeline.py::test_pipeline_runs_locally - ModuleNotFoundError: No module named 'kedro'
============================== 3 failed in 0.03s ===============================
```

</details>
