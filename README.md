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

### Data Ingestion Code Explanition
```
This code defines a data ingestion process where a dataset is read from a CSV file, split into training and testing sets, and then saved into designated paths. Let’s break it down step by step:

1. Imports
os: Used to interact with the file system, including path creation and directory management.
sys: This is included for system-specific parameters and functions. It helps handle exceptions in combination with the CustomException.
CustomException: A custom error handling class (presumably defined elsewhere) that extends Python's exception class.
logging: A logging utility (also presumably defined elsewhere) used to track events and errors in the program.
pandas (pd): A library for data manipulation and analysis, used here for reading the CSV file and saving it after processing.
train_test_split: This function from sklearn splits a dataset into training and testing sets.
dataclass: From the dataclasses module, it's used to automatically generate methods like __init__ for simple data classes.
DataTransformation: This is a placeholder import, potentially for further transformations of the data after ingestion, though it is not used in the current code.

2. DataIngestionConfig Class
The DataIngestionConfig class is decorated with @dataclass, which is a Python decorator to automatically generate an initializer (__init__) for classes with attributes.

Attributes:
train_data_path: Specifies where the training data will be saved.
test_data_path: Specifies where the testing data will be saved.
raw_data_path: Specifies where the raw data will be saved.
Paths are created using os.path.join(), which ensures that paths are correctly constructed across different operating systems.

Example paths:

artifacts/train.csv
artifacts/test.csv
artifacts/raw.csv

3. DataIngestion Class
Constructor (__init__):
Initializes the class by creating an instance of DataIngestionConfig (which stores the file paths).

initiate_data_ingestion method:
This method reads a dataset, splits it, and saves the train and test datasets. Let’s go through it line-by-line:

logging.info('Data Ingestion methods Starts'):
Logs that the data ingestion process has begun.

df=pd.read_csv('./notebooks/data/gemstone.csv'):
Reads a CSV file (gemstone.csv) and loads it into a pandas DataFrame (df).
The file is expected to be located at ./notebooks/data/gemstone.csv.

os.makedirs(os.path.dirname(self.ingestion_config.raw_data_path),exist_ok=True):
Creates the directory where the raw data will be stored if it doesn’t exist. exist_ok=True ensures no error is raised if the directory already exists.
df.to_csv(self.ingestion_config.raw_data_path,index=False):

Saves the raw data (from the DataFrame df) to the path specified by self.ingestion_config.raw_data_path (i.e., artifacts/raw.csv).
index=False prevents pandas from adding an extra index column in the CSV.
train_set,test_set=train_test_split(df,test_size=0.30):

Splits the data into two sets:
train_set: 70% of the data
test_set: 30% of the data
The train_test_split method shuffles the data and then splits it.
Saving the Train and Test Sets:

train_set.to_csv(self.ingestion_config.train_data_path,index=False,header=True):
Saves the training set to the path specified by train_data_path (artifacts/train.csv).
index=False prevents adding an index column. header=True ensures column names are written.
test_set.to_csv(self.ingestion_config.test_data_path,index=False,header=True):
Saves the testing set to artifacts/test.csv with similar parameters.
logging.info('Ingestion of data is completed'):

Logs that the data ingestion process is finished.
return (self.ingestion_config.train_data_path, self.ingestion_config.test_data_path):

Returns the paths of the train and test data files.

4. Exception Handling
If any error occurs in the process (e.g., file not found), an exception is caught in the except block.
raise CustomException(e, sys): Raises a CustomException, passing the original exception e and system information sys for additional context.

Summary
The code ingests a CSV file, splits it into training and test sets, and saves them in specified directories. Logging is used to track progress, and custom error handling is in place to manage any issues that arise during the process.
```


@app.route('/predict',methods=['GET','POST'])

def predict_datapoint():
    if request.method=='GET':
        return render_template('form.html')