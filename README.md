# Pytest

## Commands

Run pytest with verbose output
`pytest -v`

Hide warnings
`pytest -p no:warnings`

Run tests that have the custom marker slow
`pytest -m slow`
`pytest -m "not slow"`
`pytest -m "slow and fast"`
`pytest -m "slow or fast"`

Run tests that have 'logged' keyword in their name
`pytest -k "logged"`

Prints print statements to shell
`pytest -s`

List test with durations, x is the number of slowest durations
`pytest --durations=0`

List test with durations over 1 second
`pytest --durations-min=1.0`

Creates a results.xml file (e.g. for Jenkins)
`pytest --junitxml="BUILD\_${BUILD_NUMBER}\_results.xml"`

## pytest-html

Creates a results.html file with test results
`pytest --html="results.html"`

## pytest-metadata

## pytest-cov

## pytest-sugar

Makes the default pytest results better
`pip install pytest-sugar`

Run pytest without sugar
`pytest -p no:sugar`

## pytest-xdist

Run pytest with 8 threads
`pytest -n 8`

Detect processors and use all threads
`pytest -n auto`

## pytest.ini

File starts with
`[pytest]`

Run pytest with these flags
`adopts = -s -v`

Register markers

```
markers =
    slow
    performance: with description
```

## Pytest mark

`from pytest import mark`

Skip the test
`@pytest.mark.skip`

Skip the test with an added reason
`@pytest.mark.skip(reason="Some reason")`

Skip test conditionally with an added reason
`@pytest.mark.skipif(4 > 1, reason="Some reason")`

Mark test expected to fail, still run it but marked as xfail instead of fail
`@pytest.mark.xfail`
`@pytest.mark.xfail(reason="Some reason")`

Custom markers, for example mark a test as 'slow'
`@pytest.mark.slow`

Runs the test in its own transaction that will be rolled back at the end of the test, allowing parallel testing
`@pytest.mark.django_db`

## Pytest fixture

Mark a function as a fixture that can be used as a parameter in a test function
`@pytest.fixture`

Run fixture for every parameter
`@fixture(params=[])`

## Pytest parametrize

Run the test for each value in values
`@pytest.mark.parametrize(varname, ["one", "two", "three"])`

Same with multiple variables
`@pytest.mark.parametrize("var1,var2", [(0, 1), (2, 3), (4, 5)])`

Indirect=True: first run the "companies" fixture on the data before giving the data to the test
`pytest.mark.parametrize("companies", [["one", "two", "three"], ["four", "five"]], indirect=True)`

With ids
`pytest.mark.parametrize("companies", [["one", "two", "three"], ["four", "five"]], ids=["first three", "last two"], indirect=True)`

## Globals

Give every function in the file this decorator
`pytestmark = pytest.mark.django_db`

`pytestmark = [pytest.mark.django_db, pytest.mark.slow]`

## pytest.raises

```
with pytest.raises(Exception) as e:
    raise_some_exception()
assert "Some Value" == str(e.value)
```

## caplog fixture

`assert "some value" in caplog.text`

```
with caplog.at_level(logging.INFO):
    assert "infolog" in caplog.text
```

## pytest mailoutbox fixture

```
from django.core import mail
assert len(mailoutbox) == 0
mail.send(...)
assert len(mailoutbox) == 1
assert mailoutbox[0].subject == "Test subject"
```

## pytest settings fixture

Change settings with settings fixture

```
def test_something(settings);
    settings.EMAIL_BACKEND="django.core.mail.backends.locmem.EmailBackend"
```

## unittest.mock.patch

Patch replaces an object with another object

`def patch(target, new=DEFAULT, spec=None, create=False, spec_set=None, autospec=None, new_callable=None, **kwargs):`

### Function scope example

Patch target targets where the object is being used, not where it is defined.

```
# db_utils.py
def db_write():
    print("db_write, enter")

# utils.py
from db_utils import db_write

def foo()
    print("foo, enter")
    x = db_write()
    return x

# test_mocking.py
from unittest.mock import patch
from utils import foo

@patch("utils.db_write", MagicMock(return_value=42))
def test_mock_out_db_write():
    assert foo() == 42
```

Doesn't run the actual function but makes sure it is called with the given arguments

### Context scope example

```
from unittest.mock import patch

with patch("path.to.function") as mocked_function:
    mocked_function.assert_called_with(**kwargs)
```

Mock an API json response

```
responses.add(method=responses.GET, url="someurl", json={}, status=200)

response = request.get(url="url", headers={})
assert response.status_code == 200
response_content = json.loads(response.content)
assert response_content["somevalue"] == "somevalue
```

# allure-pytest

Install Allure for pytest
`pip install allure-pytest`

Install Allure (Mac)
`brew install allure`

Install Allure (Linux)

```
sudo apt-add-repository ppa:qameta/allure
sudo apt-get update
sudo apt-get install allure
```

Run pytest and create Allure json test files
`pytest . --aluredir=/tmp/my_allure_results`

Run Allure to display the created json test files
`allure serve /tmp/my_allure/results`

# other

`@fixture(params[webdriver.Chome, webdriver.Firefox]) def browser(request): driver = request.param drvr = driver() yield drvr drvr.quit()`

Tox is a tool for running tests in different virtual environments.
python -m pip install tox
tox # creates tox.ini

[pytest]

pytest --markers # prints markers with descriptions that are documented in pytest.ini

## django-waffle

Feature flags to use new code on only a percentage of the live database.

## functools.lru_cache

Use lru_cache decorator on functions that take a long time and the input-output is always the same.
`@lru_cache(maxsize=256)`
