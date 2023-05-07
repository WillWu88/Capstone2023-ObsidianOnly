#programming 

## Topics

#### 1. Super Init

- Super init
	- [Reference](https://stackoverflow.com/questions/576169/understanding-python-super-with-init-methods)
	- Note: 
	- Code example
```python
def __init__(self):
	super().__init__()
```

#### 2. Python Build System

- Important pieces: `setup.py`, `setup.cfg`, `__init__.py`, `__main__.py`
	- `__init.py__` recognize directory as packages

- Include files in a python project
	- File strucutre:
```Shell
➜  picar_sys git:(main) ✗ tree ..
..
├── package.xml
├── picar_sys
│   ├── encoder.py
│   ├── imu_driver.py
│   ├── imu_param.py
│   ├── imu.py
│   ├── __init__.py
│   └── __pycache__
│       ├── imu_driver.cpython-310.pyc
│       └── imu_param.cpython-310.pyc
├── resource
│   └── picar_sys
├── setup.cfg
├── setup.py
└── test
    ├── test_copyright.py
    ├── test_flake8.py
    └── test_pep257.py
```

- Include package as follows:
```python
import picar_sys.imu_driver
# note the prepended picar_sys 
```
- [Documentation]([python - What is __init__.py for? - Stack Overflow](https://stackoverflow.com/questions/448271/what-is-init-py-for))

#### 3. Specifying return types

```python
def function(arg) -> return_class:
	pass
```

