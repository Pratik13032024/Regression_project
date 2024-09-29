## End to end ML project

### Create a virtual enviroment
```
conda create -p venv python=3.8
```

### Create a new file name as requirements.txt and install all the necessary labraries
```
once you add any package name into requiremnet file then you can install it by below command

pip install -r requirements.txt
```

### Create a new file name as Setup.py and write a code to install all the req and install a colletcted packages (RegressorProject.egg-info)
```
------------------------Command---------------------------------
python setup.py install
----------------------------------------------------------------

from setuptools import find_packages,setup
from typing import List

HYPEN_E_DOT='-e .'

def get_requirements(file_path:str)->List[str]:
    requirements=[]
    with open(file_path) as file_obj:
        requirements=file_obj.readlines()
        requirements=[req.replace("\n","") for req in requirements]

        if HYPEN_E_DOT in requirements:
            requirements.remove(HYPEN_E_DOT)

    return requirements



setup(
    name='RegressorProject',
    version='0.0.1',
    author='Pratik',
    author_email='dombale22@gmail.com',
    install_requires=get_requirements('requirements.txt'),
    packages=find_packages()
)

```