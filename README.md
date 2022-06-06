# Machine-Learning-With-Python_-Assignment
Peer Graded Assignment, Coursera
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "<p style=\"text-align:center\">\n",
    "    <a href=\"https://skills.network/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork20718538-2022-01-01\" target=\"_blank\">\n",
    "    <img src=\"https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png\" width=\"200\" alt=\"Skills Network Logo\"  />\n",
    "    </a>\n",
    "</p>\n",
    "\n",
    "<h1 align=\"center\"><font size=\"5\">Classification with Python</font></h1>\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "In this notebook we try to practice all the classification algorithms that we have learned in this course.\n",
    "\n",
    "We load a dataset using Pandas library, and apply the following algorithms, and find the best one for this specific dataset by accuracy evaluation methods.\n",
    "\n",
    "Let's first load required libraries:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [],
   "source": [
    "import itertools\n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "from matplotlib.ticker import NullFormatter\n",
    "import pandas as pd\n",
    "import numpy as np\n",
    "import matplotlib.ticker as ticker\n",
    "from sklearn import preprocessing\n",
    "%matplotlib inline"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### About dataset\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "This dataset is about past loans. The **Loan_train.csv** data set includes details of 346 customers whose loan are already paid off or defaulted. It includes following fields:\n",
    "\n",
    "| Field          | Description                                                                           |\n",
    "| -------------- | ------------------------------------------------------------------------------------- |\n",
    "| Loan_status    | Whether a loan is paid off on in collection                                           |\n",
    "| Principal      | Basic principal loan amount at the                                                    |\n",
    "| Terms          | Origination terms which can be weekly (7 days), biweekly, and monthly payoff schedule |\n",
    "| Effective_date | When the loan got originated and took effects                                         |\n",
    "| Due_date       | Since it’s one-time payoff schedule, each loan has one single due date                |\n",
    "| Age            | Age of applicant                                                                      |\n",
    "| Education      | Education of applicant                                                                |\n",
    "| Gender         | The gender of applicant                                                               |\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Let's download the dataset\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "--2022-06-06 20:06:20--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-ML0101EN-SkillsNetwork/labs/FinalModule_Coursera/data/loan_train.csv\n",
      "Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104\n",
      "Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.\n",
      "HTTP request sent, awaiting response... 200 OK\n",
      "Length: 23101 (23K) [text/csv]\n",
      "Saving to: ‘loan_train.csv’\n",
      "\n",
      "loan_train.csv      100%[===================>]  22.56K  --.-KB/s    in 0s      \n",
      "\n",
      "2022-06-06 20:06:20 (284 MB/s) - ‘loan_train.csv’ saved [23101/23101]\n",
      "\n"
     ]
    }
   ],
   "source": [
    "!wget -O loan_train.csv https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-ML0101EN-SkillsNetwork/labs/FinalModule_Coursera/data/loan_train.csv"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Load Data From CSV File\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Unnamed: 0</th>\n",
       "      <th>Unnamed: 0.1</th>\n",
       "      <th>loan_status</th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>effective_date</th>\n",
       "      <th>due_date</th>\n",
       "      <th>age</th>\n",
       "      <th>education</th>\n",
       "      <th>Gender</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>9/8/2016</td>\n",
       "      <td>10/7/2016</td>\n",
       "      <td>45</td>\n",
       "      <td>High School or Below</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>2</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>9/8/2016</td>\n",
       "      <td>10/7/2016</td>\n",
       "      <td>33</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>female</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>3</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>15</td>\n",
       "      <td>9/8/2016</td>\n",
       "      <td>9/22/2016</td>\n",
       "      <td>27</td>\n",
       "      <td>college</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>4</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>9/9/2016</td>\n",
       "      <td>10/8/2016</td>\n",
       "      <td>28</td>\n",
       "      <td>college</td>\n",
       "      <td>female</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>6</td>\n",
       "      <td>6</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>9/9/2016</td>\n",
       "      <td>10/8/2016</td>\n",
       "      <td>29</td>\n",
       "      <td>college</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Unnamed: 0  Unnamed: 0.1 loan_status  Principal  terms effective_date  \\\n",
       "0           0             0     PAIDOFF       1000     30       9/8/2016   \n",
       "1           2             2     PAIDOFF       1000     30       9/8/2016   \n",
       "2           3             3     PAIDOFF       1000     15       9/8/2016   \n",
       "3           4             4     PAIDOFF       1000     30       9/9/2016   \n",
       "4           6             6     PAIDOFF       1000     30       9/9/2016   \n",
       "\n",
       "    due_date  age             education  Gender  \n",
       "0  10/7/2016   45  High School or Below    male  \n",
       "1  10/7/2016   33              Bechalor  female  \n",
       "2  9/22/2016   27               college    male  \n",
       "3  10/8/2016   28               college  female  \n",
       "4  10/8/2016   29               college    male  "
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df = pd.read_csv('loan_train.csv')\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(346, 10)"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Convert to date time object\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Unnamed: 0</th>\n",
       "      <th>Unnamed: 0.1</th>\n",
       "      <th>loan_status</th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>effective_date</th>\n",
       "      <th>due_date</th>\n",
       "      <th>age</th>\n",
       "      <th>education</th>\n",
       "      <th>Gender</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-10-07</td>\n",
       "      <td>45</td>\n",
       "      <td>High School or Below</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>2</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-10-07</td>\n",
       "      <td>33</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>female</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>3</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>15</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-09-22</td>\n",
       "      <td>27</td>\n",
       "      <td>college</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>4</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-09</td>\n",
       "      <td>2016-10-08</td>\n",
       "      <td>28</td>\n",
       "      <td>college</td>\n",
       "      <td>female</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>6</td>\n",
       "      <td>6</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-09</td>\n",
       "      <td>2016-10-08</td>\n",
       "      <td>29</td>\n",
       "      <td>college</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Unnamed: 0  Unnamed: 0.1 loan_status  Principal  terms effective_date  \\\n",
       "0           0             0     PAIDOFF       1000     30     2016-09-08   \n",
       "1           2             2     PAIDOFF       1000     30     2016-09-08   \n",
       "2           3             3     PAIDOFF       1000     15     2016-09-08   \n",
       "3           4             4     PAIDOFF       1000     30     2016-09-09   \n",
       "4           6             6     PAIDOFF       1000     30     2016-09-09   \n",
       "\n",
       "    due_date  age             education  Gender  \n",
       "0 2016-10-07   45  High School or Below    male  \n",
       "1 2016-10-07   33              Bechalor  female  \n",
       "2 2016-09-22   27               college    male  \n",
       "3 2016-10-08   28               college  female  \n",
       "4 2016-10-08   29               college    male  "
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df['due_date'] = pd.to_datetime(df['due_date'])\n",
    "df['effective_date'] = pd.to_datetime(df['effective_date'])\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "# Data visualization and pre-processing\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Let’s see how many of each class is in our data set\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "PAIDOFF       260\n",
       "COLLECTION     86\n",
       "Name: loan_status, dtype: int64"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df['loan_status'].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "260 people have paid off the loan on time while 86 have gone into collection\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Let's plot some columns to underestand data better:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Requirement already satisfied: seaborn in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (0.9.0)\n",
      "Requirement already satisfied: scipy>=0.14.0 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from seaborn) (1.7.3)\n",
      "Requirement already satisfied: pandas>=0.15.2 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from seaborn) (1.3.5)\n",
      "Requirement already satisfied: matplotlib>=1.4.3 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from seaborn) (3.5.2)\n",
      "Requirement already satisfied: numpy>=1.9.3 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from seaborn) (1.21.6)\n",
      "Requirement already satisfied: python-dateutil>=2.7 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from matplotlib>=1.4.3->seaborn) (2.8.2)\n",
      "Requirement already satisfied: packaging>=20.0 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from matplotlib>=1.4.3->seaborn) (21.3)\n",
      "Requirement already satisfied: cycler>=0.10 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from matplotlib>=1.4.3->seaborn) (0.11.0)\n",
      "Requirement already satisfied: pyparsing>=2.2.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from matplotlib>=1.4.3->seaborn) (3.0.9)\n",
      "Requirement already satisfied: pillow>=6.2.0 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from matplotlib>=1.4.3->seaborn) (8.1.0)\n",
      "Requirement already satisfied: kiwisolver>=1.0.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from matplotlib>=1.4.3->seaborn) (1.4.2)\n",
      "Requirement already satisfied: fonttools>=4.22.0 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from matplotlib>=1.4.3->seaborn) (4.33.3)\n",
      "Requirement already satisfied: pytz>=2017.3 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from pandas>=0.15.2->seaborn) (2022.1)\n",
      "Requirement already satisfied: typing-extensions in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from kiwisolver>=1.0.1->matplotlib>=1.4.3->seaborn) (4.2.0)\n",
      "Requirement already satisfied: six>=1.5 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from python-dateutil>=2.7->matplotlib>=1.4.3->seaborn) (1.16.0)\n",
      "Note: you may need to restart the kernel to use updated packages.\n",
      "Requirement already satisfied: scikit-learn==0.23.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (0.23.1)\n",
      "Requirement already satisfied: scipy>=0.19.1 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from scikit-learn==0.23.1) (1.7.3)\n",
      "Requirement already satisfied: numpy>=1.13.3 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from scikit-learn==0.23.1) (1.21.6)\n",
      "Requirement already satisfied: joblib>=0.11 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from scikit-learn==0.23.1) (1.1.0)\n",
      "Requirement already satisfied: threadpoolctl>=2.0.0 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (from scikit-learn==0.23.1) (3.1.0)\n",
      "Note: you may need to restart the kernel to use updated packages.\n"
     ]
    }
   ],
   "source": [
    "# notice: installing seaborn might takes a few minutes\n",
    "%pip install seaborn\n",
    "%pip install scikit-learn==0.23.1"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAagAAADQCAYAAABStPXYAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjUuMiwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8qNh9FAAAACXBIWXMAAAsTAAALEwEAmpwYAAAbEElEQVR4nO3de5xVdb3/8ddbnBwRzQuTIoQzIpIg/HY6aWZ2EJXIC+SxFDOTjueQphYnzULLOvnITEirY2p4Inx0BKWLaFheIjiG5QVxVFDB26Sj3O2RkkCAn98fe824wT3MZe89s2bv9/PxWI+91nevy2cx+8tnf79r7fVVRGBmZpY2O3V3AGZmZvk4QZmZWSo5QZmZWSo5QZmZWSo5QZmZWSo5QZmZWSo5QZWIpH0lzZT0oqTHJP1F0qlF2vdISXOLsa+uIGmBpPrujsO6RznVBUk1kh6W9LikY0p4nPWl2ndP4gRVApIEzAEeiIgDI+JwYDwwoJvi2bk7jmtWhnXhOODZiPhgRPypGDFZ65ygSmMU8M+IuKm5ICL+GhH/DSCpl6Qpkh6V9KSkLyTlI5PWxq8kPSvp1qSCI2lMUrYQ+Nfm/UraTdL0ZF+PSxqXlE+Q9EtJvwXuK+RkJM2QdKOk+cm34H9JjvmMpBk5690oaZGkpZL+q5V9jU6+QS9O4utTSGyWemVTFyRlgGuAEyU1SNq1tc+zpEZJVyXvLZJ0mKR7Jb0g6bxknT6S5iXbPtUcb57jfjXn3ydvvSpbEeGpyBPwJeC6Hbw/EfhGMr8LsAioA0YCfyf77XIn4C/AR4Fq4BVgMCBgNjA32f4q4LPJ/J7AcmA3YALQBOzdSgx/AhryTMfnWXcGcFty7HHAG8DwJMbHgEyy3t7Jay9gATAiWV4A1AN9gQeA3ZLyrwFXdPffy1PppjKsCxOA65P5Vj/PQCNwfjJ/HfAksDtQA6xOyncG9sjZ1/OAkuX1yetoYFpyrjsBc4GPdffftasmd/10AUk/IVu5/hkRHyL7oRsh6VPJKu8lW+H+CTwSEU3Jdg1ALbAeeCkinkvK/5dsxSbZ11hJlyTL1cDAZP7+iHg9X0wR0dH+899GREh6ClgVEU8lsSxNYmwATpc0kWzF6wcMJVsxm304KXsw+TL8HrL/8ViFKJO60Kytz/NdyetTQJ+IeBN4U9JGSXsC/wCukvQx4G2gP7AvsDJnH6OT6fFkuQ/Zf58HOhlzj+IEVRpLgdOaFyLiAkl9yX47hOy3oYsi4t7cjSSNBDblFG3lnb9Raw9NFHBaRCzbbl9Hkq0A+TeS/kT2G932LomIP+Qpb47r7e1ifBvYWVIdcAnwoYj4W9L1V50n1vsj4szW4rKyU451Ifd4O/o877DOAGeRbVEdHhGbJTWSv858LyJ+uoM4ypavQZXGH4FqSefnlPXOmb8XOF9SFYCkgyXttoP9PQvUSRqULOdWiHuBi3L65z/YngAj4piIyOSZdlQhd2QPsv8J/F3SvsAn8qzzEHC0pIOSWHtLOriTx7OeoZzrQqGf5/eS7e7bLOlY4IA869wL/FvOta3+kt7XgWP0aE5QJRDZzuNPAv8i6SVJjwC3kO2jBvgf4GlgsaQlwE/ZQWs2IjaS7ca4O7kw/Nect68EqoAnk31dWeTTaZeIeIJsN8RSYDrwYJ511pDtw58l6UmyFfwDXRimdbFyrgtF+DzfCtRLWkS2NfVsnmPcB8wE/pJ0r/+K/K29stR8Qc7MzCxV3IIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUSkWCGjNmTJD9bYMnT+UyFY3rh6cym9otFQlq7dq13R2CWWq5flilSkWCMjMz254TlJmZpZITlJmZpZIfFmtmZWXz5s00NTWxcePG7g6lolVXVzNgwACqqqo6vQ8nKDMrK01NTey+++7U1taSPDfWulhEsG7dOpqamqirq+v0ftzFZ2ZlZePGjeyzzz5OTt1IEvvss0/BrVgnKKsYB/Trh6SCpwP69evuU7E2ODl1v2L8DdzFZxXj5ZUradp/QMH7GfBaUxGiMbO2uAVlZmWtWC3njrSge/XqRSaT4dBDD+XTn/40b731FgBbtmyhb9++TJ48eZv1R44cyaJF2UGGa2trGT58OMOHD2fo0KF84xvfYNOmdwbkXbp0KaNGjeLggw9m8ODBXHnllTQPmzRjxgxqamrIZDJkMhk+97nPATBhwgTq6upayn/84x8X5d+21NyCMrOyVqyWc7P2tKB33XVXGhoaADjrrLO46aab+MpXvsJ9993HkCFDmD17NldddVWr3WDz58+nb9++rF+/nokTJzJx4kRuueUWNmzYwNixY7nxxhsZPXo0b731Fqeddho33HADF1xwAQBnnHEG119//bv2OWXKFD71qU91/sS7QZstKEnTJa1ORqhsLvu2pFclNSTTiTnvTZb0vKRlkj5eqsDNzHqCY445hueffx6AWbNm8eUvf5mBAwfy0EMPtbltnz59uOmmm5gzZw6vv/46M2fO5Oijj2b06NEA9O7dm+uvv56rr766pOfQXdrTxTcDGJOn/LqIyCTT7wAkDQXGA8OSbW6Q1KtYwZqZ9SRbtmzh97//PcOHD2fDhg3MmzePk08+mTPPPJNZs2a1ax977LEHdXV1PPfccyxdupTDDz98m/cHDRrE+vXreeONNwC4/fbbW7ryfv7zn7es99WvfrWl/KmnnireSZZQmwkqIh4AXm/n/sYBt0XEpoh4CXgeOKKA+MzMepwNGzaQyWSor69n4MCBnHvuucydO5djjz2W3r17c9ppp3HHHXewdevWdu2v+RpTRLTaLdhcfsYZZ9DQ0EBDQwOf//znW96fMmVKS/nw4cMLPMOuUcg1qAslfQ5YBFwcEX8D+gO57dampOxdJE0EJgIMHDiwgDDMyo/rR8+Wew2q2axZs3jwwQepra0FYN26dcyfP5/jjz9+h/t68803aWxs5OCDD2bYsGE88MAD27z/4osv0qdPH3bfffdinkIqdPYuvhuBQUAGWAH8ICnPl9rzjv8REdMioj4i6mtqajoZhll5cv0oL2+88QYLFy7k5ZdfprGxkcbGRn7yk5+02c23fv16vvjFL/LJT36Svfbai7POOouFCxfyhz/8Aci21L70pS9x6aWXdsVpdLlOtaAiYlXzvKSbgbnJYhPw/pxVBwCvdTo6M7MCDdxvv6L+dm3gfvt1eJvf/OY3jBo1il122aWlbNy4cVx66aXb3ELe7NhjjyUiePvttzn11FP55je/CWRbZnfeeScXXXQRF1xwAVu3buXss8/mwgsv7PwJpZia+zZ3uJJUC8yNiEOT5X4RsSKZ/0/gyIgYL2kYMJPsdaf9gXnA4IjYYUdrfX19NP8GwKxUJBXth7rtqDdFe5SB60fHPPPMMxxyyCHdHYbR6t+i3XWjzRaUpFnASKCvpCbgW8BISRmy3XeNwBcAImKppNnA08AW4IK2kpOZmVk+bSaoiDgzT/HPdrD+d4HvFhKUmZmZH3VkZmap5ARlZmap5ARlZmap5ARlZmap5ARlZmVt/wEDizrcxv4D2n6yx8qVKxk/fjyDBg1i6NChnHjiiSxfvrzNoTLy/Z6ptraWtWvXblO2/bAamUyGp59+GoDly5dz4oknctBBB3HIIYdw+umnb/N8vj59+jBkyJCW4TgWLFjAySef3LLvOXPmMGLECD7wgQ8wfPhw5syZ0/LehAkT6N+/f8tvt9auXdvyZIxS8HAbZlbWVrz6CkdecU/R9vfwd/I9O/sdEcGpp57KOeecw2233QZAQ0MDq1atYsKECTscKqMj8g2rsXHjRk466SSuvfZaTjnlFCA7dEdNTU3Lo5dGjhzJ1KlTqa+vB2DBggUt2z/xxBNccskl3H///dTV1fHSSy9xwgkncOCBBzJixAggO9bV9OnTOf/88zscc0e5BWVmVkTz58+nqqqK8847r6Usk8mwfPnykg+VMXPmTI466qiW5ATZp1Iceuih7dp+6tSpXHbZZdTV1QFQV1fH5MmTmTJlSss6kyZN4rrrrmPLli1Fi7s1TlBmZkW0ZMmSdw2JAbRrqIyOyO22y2QybNiwodVjt1e+GOvr61m6dGnL8sCBA/noRz/KL37xi04fp73cxWdm1gXaM1RGR7Q2cm4h8sWYr+yyyy5j7NixnHTSSUU9/vbcgjIzK6Jhw4bx2GOP5S3f/pmKxR4qo7Vjd2T77WNcvHgxQ4cO3absoIMOIpPJMHv27E4fqz2coMzMimjUqFFs2rSJm2++uaXs0UcfZfDgwSUfKuMzn/kMf/7zn7n77rtbyu655552j6B7ySWX8L3vfY/GxkYAGhsbueqqq7j44ovfte7ll1/O1KlTixJ3a9zFZ2ZlrV//97d5511H97cjkrjjjjuYNGkSV199NdXV1dTW1vLDH/6wzaEyZsyYsc1t3Q89lB3/dcSIEey0U7Y9cfrppzNixAhuv/12Fi5c2LLuDTfcwEc+8hHmzp3LpEmTmDRpElVVVYwYMYIf/ehH7Tq3TCbD97//fU455RQ2b95MVVUV11xzDZlM5l3rDhs2jMMOO4zFixe3a9+d0a7hNkrNwwlYV/BwG5XBw22kR6HDbbTZxSdpuqTVkpbklE2R9KykJyXdIWnPpLxW0gZJDcl0U3sDMTMzy9Wea1AzgO3bx/cDh0bECGA5MDnnvRciIpNM52FmZtYJbSaoiHgAeH27svsiovlXWg+RHdrdzCwV0nDpotIV429QjLv4/g34fc5ynaTHJf2fpGNa20jSREmLJC1as2ZNEcIwKx+uH51XXV3NunXrnKS6UUSwbt06qqurC9pPQXfxSbqc7NDutyZFK4CBEbFO0uHAHEnDIuJdP5OOiGnANMheBC4kDrNy4/rReQMGDKCpqQkn9u5VXV3NgAGFda51OkFJOgc4GTgukq8qEbEJ2JTMPybpBeBgwLcgmVmXqKqqanmWnPVsnerikzQG+BowNiLeyimvkdQrmT8QGAy8WIxAzcyssrTZgpI0CxgJ9JXUBHyL7F17uwD3J89oeii5Y+9jwHckbQG2AudFxOt5d2xmZrYDbSaoiDgzT/HPWln318CvCw3KzMzMz+IzM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUcoIyM7NUajNBSZouabWkJTlle0u6X9JzyeteOe9NlvS8pGWSPl6qwM3MrLy1pwU1AxizXdnXgXkRMRiYlywjaSgwHhiWbHND8wi7ZmZmHdFmgoqIB4DtR8UdB9ySzN8CfDKn/LaI2BQRLwHPA0cUJ1QzM6sknb0GtW9ErABIXt+XlPcHXslZrykpexdJEyUtkrRozZo1nQzDrDy5fpgV/yYJ5SmLfCtGxLSIqI+I+pqamiKHYdazuX6YdT5BrZLUDyB5XZ2UNwHvz1lvAPBa58MzM7NK1dkEdRdwTjJ/DnBnTvl4SbtIqgMGA48UFqKZmVWindtaQdIsYCTQV1IT8C3gamC2pHOBl4FPA0TEUkmzgaeBLcAFEbG1RLGbmVkZazNBRcSZrbx1XCvrfxf4biFBmZmZ+UkSZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSk5QZmaWSm0+zbw1koYAt+cUHQhcAewJ/AfQPE71ZRHxu84ex8zMKlOnE1RELAMyAJJ6Aa8CdwCfB66LiKnFCNDMzCpTsbr4jgNeiIi/Fml/ZmZW4YqVoMYDs3KWL5T0pKTpkvbKt4GkiZIWSVq0Zs2afKuYVSzXD7MiJChJ7wHGAr9Mim4EBpHt/lsB/CDfdhExLSLqI6K+pqam0DDMyorrh1lxWlCfABZHxCqAiFgVEVsj4m3gZuCIIhzDzMwqTDES1JnkdO9J6pfz3qnAkiIcw8zMKkyn7+IDkNQbOAH4Qk7xNZIyQACN271nZmbWLgUlqIh4C9hnu7KzC4rIzMwMP0nCzMxSygnKzMxSyQnKzMxSyQnKzMxSyQnKzMxSyQnKzMxSqaDbzM16EvWqYsBrTUXZj5mVnhOUVYzYupkjr7in4P08/J0xRYjGzNriLj4zM0slJygzM0slJygzM0slJygzM0slJygzM0slJygzM0ulQseDagTeBLYCWyKiXtLewO1ALdnxoE6PiL8VFqaZmVWaYrSgjo2ITETUJ8tfB+ZFxGBgXrJsFeiAfv2QVPB0QL9+bR/MzMpOKX6oOw4YmczfAiwAvlaC41jKvbxyJU37Dyh4P8V4+oOZ9TyFtqACuE/SY5ImJmX7RsQKgOT1ffk2lDRR0iJJi9asWVNgGGblxfXDrPAEdXREHAZ8ArhA0sfau2FETIuI+oior6mpKTAMs/Li+mFWYIKKiNeS19XAHcARwCpJ/QCS19WFBmlmZpWn0wlK0m6Sdm+eB0YDS4C7gHOS1c4B7iw0SDMzqzyF3CSxL3CHpOb9zIyIeyQ9CsyWdC7wMvDpwsM0M7NK0+kEFREvAv8vT/k64LhCgjIzM/OTJMzMLJWcoMzMLJWcoMzMLJWcoMzMLJWcoMzMLJWcoMzMLJWcoMzMLJWcoMzMLJWcoMzMLJWcoMzMLJWcoMzMKlyxRr8u9gjYpRhR18zMepBijX4NxR0B2y0oMzNLpULGg3q/pPmSnpG0VNKXk/JvS3pVUkMynVi8cM3MrFIU0sW3Bbg4IhYnAxc+Jun+5L3rImJq4eGZmVmlKmQ8qBXAimT+TUnPAP2LFZiZmVW2olyDklQLfBB4OCm6UNKTkqZL2quVbSZKWiRp0Zo1a4oRhlnZcP0wK0KCktQH+DUwKSLeAG4EBgEZsi2sH+TbLiKmRUR9RNTX1NQUGoZZWXH9MCswQUmqIpucbo2I3wBExKqI2BoRbwM3A0cUHqaZmVWaQu7iE/Az4JmIuDanPPdXWqcCSzofnpmZVapC7uI7GjgbeEpSQ1J2GXCmpAwQQCPwhQKOYWZmFaqQu/gWAsrz1u86H46ZmVmWnyRhZmap5GfxWcmoV1VRnsulXlVFiMbMehonKCuZ2LqZI6+4p+D9PPydMUWIxsx6GnfxmZlZKjlBmZlZKjlBmZlZKjlBmZlZKjlBmZl1sWINsV7M4dXTyHfxmZl1sWINsV7M4dXTyC0oMzNLJScoMzNLJXfxmZlVuGI99aV5X8XiBGVmVuGK9dQXKO6TX9zFZ2ZmqVSyBCVpjKRlkp6X9PVC9+fbMs3MKktJuvgk9QJ+ApwANAGPSrorIp7u7D59W6aZWWUp1TWoI4DnI+JFAEm3AeOATieotDmgXz9eXrmy4P0M3G8//rpiRREiKm9SvrExLY1cN9pWrJsSdupVVdZ1QxFR/J1KnwLGRMS/J8tnA0dGxIU560wEJiaLQ4BlRQ+k/foCa7vx+IVw7F2vPXGvjYhOXy1OUf3oqX8jcOzdpa3Y2103StWCypfSt8mEETENmFai43eIpEURUd/dcXSGY+96XRF3WupHT/0bgWPvLsWMvVQ3STQB789ZHgC8VqJjmZlZGSpVgnoUGCypTtJ7gPHAXSU6lpmZlaGSdPFFxBZJFwL3Ar2A6RGxtBTHKpJu70opgGPvej017s7oyefq2LtH0WIvyU0SZmZmhfKTJMzMLJWcoMzMLJUqJkFJ6iXpcUlzk+W9Jd0v6bnkda+cdScnj2haJunj3Rc1SNpT0q8kPSvpGUlH9aDY/1PSUklLJM2SVJ3W2CVNl7Ra0pKcsg7HKulwSU8l7/1YPeBXlK4b3RK760Z76kZEVMQEfAWYCcxNlq8Bvp7Mfx34fjI/FHgC2AWoA14AenVj3LcA/57MvwfYsyfEDvQHXgJ2TZZnAxPSGjvwMeAwYElOWYdjBR4BjiL7W8DfA5/ors9OB87ddaNr43bdaGfd6PbK0UX/wAOAecConEq4DOiXzPcDliXzk4HJOdveCxzVTXHvkXyQtV15T4i9P/AKsDfZu0XnAqPTHDtQu10l7FCsyTrP5pSfCfy0O/79O3DOrhtdH7vrRjvrRqV08f0QuBR4O6ds34hYAZC8vi8pb/7wNGtKyrrDgcAa4OdJF8z/SNqNHhB7RLwKTAVeBlYAf4+I++gBsefoaKz9k/nty9Psh7hudCnXjW3Kd6jsE5Skk4HVEfFYezfJU9Zd9+LvTLZpfWNEfBD4B9nmdGtSE3vSJz2ObDN/f2A3SZ/d0SZ5ytL6G4jWYu1J5+C64bpRCkWtG2WfoICjgbGSGoHbgFGS/hdYJakfQPK6Olk/TY9pagKaIuLhZPlXZCtlT4j9eOCliFgTEZuB3wAfoWfE3qyjsTYl89uXp5XrRvdw3WjnOZR9goqIyRExICJqyT5y6Y8R8Vmyj146J1ntHODOZP4uYLykXSTVAYPJXtzrchGxEnhF0pCk6DiyQ5akPnay3RcfltQ7uVvnOOAZekbszToUa9LV8aakDyfn/LmcbVLHdcN1owBdUze64yJhd03ASN65ELwP2YvDzyWve+esdznZu0+W0c13YQEZYBHwJDAH2KsHxf5fwLPAEuAXZO/sSWXswCyy1wM2k/22d25nYgXqk/N9Abie7S7ip3Vy3ejy2F032lE3/KgjMzNLpbLv4jMzs57JCcrMzFLJCcrMzFLJCcrMzFLJCcrMzFLJCSrFJG2V1JA88fiXknq3st6fO7n/ekk/LiC+9Z3d1qwQrhuVwbeZp5ik9RHRJ5m/FXgsIq7Neb9XRGxNQ3xmXcl1ozK4BdVz/Ak4SNJISfMlzQSegne+rSXvLdA7Y+Tc2jzmiqQPSfqzpCckPSJp92T95jGAvi3pF5L+mIzx8h9JeR9J8yQtTsZyGdc9p2/WKteNMrVzdwdgbZO0M/AJ4J6k6Ajg0Ih4Kc/qHwSGkX3O1YPA0ZIeAW4HzoiIRyXtAWzIs+0I4MPAbsDjku4m+4ytUyPiDUl9gYck3RVuelsKuG6UN7eg0m1XSQ1kH+fyMvCzpPyRVipg83tNEfE20EB2HJchwIqIeBQgIt6IiC15tr0zIjZExFpgPtnKLuAqSU8CfyD7iPx9i3FyZgVw3agAbkGl24aIyOQWJL0S/9jBNpty5reS/RuL9j2ef/t1AjgLqAEOj4jNyj75urod+zIrJdeNCuAWVGV4Fthf0ocAkj72fF9OxkmqlrQP2YeHPgq8l+yYQZslHQsc0FVBm3UB140UcwuqAkTEPyWdAfy3pF3J9rEfn2fVR4C7gYHAlRHxWnKH1G8lLSLbLfJsF4VtVnKuG+nm28wNyN6pBKyPiKndHYtZmrhudB938ZmZWSq5BWVmZqnkFpSZmaWSE5SZmaWSE5SZmaWSE5SZmaWSE5SZmaXS/wcYrwBshU/c+gAAAABJRU5ErkJggg==",
      "text/plain": [
       "<Figure size 432x216 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "import seaborn as sns\n",
    "\n",
    "bins = np.linspace(df.Principal.min(), df.Principal.max(), 10)\n",
    "g = sns.FacetGrid(df, col=\"Gender\", hue=\"loan_status\", palette=\"Set1\", col_wrap=2)\n",
    "g.map(plt.hist, 'Principal', bins=bins, ec=\"k\")\n",
    "\n",
    "g.axes[-1].legend()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "image/png": "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",
      "text/plain": [
       "<Figure size 432x216 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "bins = np.linspace(df.age.min(), df.age.max(), 10)\n",
    "g = sns.FacetGrid(df, col=\"Gender\", hue=\"loan_status\", palette=\"Set1\", col_wrap=2)\n",
    "g.map(plt.hist, 'age', bins=bins, ec=\"k\")\n",
    "\n",
    "g.axes[-1].legend()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "# Pre-processing:  Feature selection/extraction\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Let's look at the day of the week people get the loan\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "image/png": "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",
      "text/plain": [
       "<Figure size 432x216 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "df['dayofweek'] = df['effective_date'].dt.dayofweek\n",
    "bins = np.linspace(df.dayofweek.min(), df.dayofweek.max(), 10)\n",
    "g = sns.FacetGrid(df, col=\"Gender\", hue=\"loan_status\", palette=\"Set1\", col_wrap=2)\n",
    "g.map(plt.hist, 'dayofweek', bins=bins, ec=\"k\")\n",
    "g.axes[-1].legend()\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "We see that people who get the loan at the end of the week don't pay it off, so let's use Feature binarization to set a threshold value less than day 4\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Unnamed: 0</th>\n",
       "      <th>Unnamed: 0.1</th>\n",
       "      <th>loan_status</th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>effective_date</th>\n",
       "      <th>due_date</th>\n",
       "      <th>age</th>\n",
       "      <th>education</th>\n",
       "      <th>Gender</th>\n",
       "      <th>dayofweek</th>\n",
       "      <th>weekend</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-10-07</td>\n",
       "      <td>45</td>\n",
       "      <td>High School or Below</td>\n",
       "      <td>male</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>2</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-10-07</td>\n",
       "      <td>33</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>female</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>3</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>15</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-09-22</td>\n",
       "      <td>27</td>\n",
       "      <td>college</td>\n",
       "      <td>male</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>4</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-09</td>\n",
       "      <td>2016-10-08</td>\n",
       "      <td>28</td>\n",
       "      <td>college</td>\n",
       "      <td>female</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>6</td>\n",
       "      <td>6</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-09</td>\n",
       "      <td>2016-10-08</td>\n",
       "      <td>29</td>\n",
       "      <td>college</td>\n",
       "      <td>male</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Unnamed: 0  Unnamed: 0.1 loan_status  Principal  terms effective_date  \\\n",
       "0           0             0     PAIDOFF       1000     30     2016-09-08   \n",
       "1           2             2     PAIDOFF       1000     30     2016-09-08   \n",
       "2           3             3     PAIDOFF       1000     15     2016-09-08   \n",
       "3           4             4     PAIDOFF       1000     30     2016-09-09   \n",
       "4           6             6     PAIDOFF       1000     30     2016-09-09   \n",
       "\n",
       "    due_date  age             education  Gender  dayofweek  weekend  \n",
       "0 2016-10-07   45  High School or Below    male          3        0  \n",
       "1 2016-10-07   33              Bechalor  female          3        0  \n",
       "2 2016-09-22   27               college    male          3        0  \n",
       "3 2016-10-08   28               college  female          4        1  \n",
       "4 2016-10-08   29               college    male          4        1  "
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df['weekend'] = df['dayofweek'].apply(lambda x: 1 if (x>3)  else 0)\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "## Convert Categorical features to numerical values\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Let's look at gender:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Gender  loan_status\n",
       "female  PAIDOFF        0.865385\n",
       "        COLLECTION     0.134615\n",
       "male    PAIDOFF        0.731293\n",
       "        COLLECTION     0.268707\n",
       "Name: loan_status, dtype: float64"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.groupby(['Gender'])['loan_status'].value_counts(normalize=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "86 % of female pay there loans while only 73 % of males pay there loan\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Let's convert male to 0 and female to 1:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Unnamed: 0</th>\n",
       "      <th>Unnamed: 0.1</th>\n",
       "      <th>loan_status</th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>effective_date</th>\n",
       "      <th>due_date</th>\n",
       "      <th>age</th>\n",
       "      <th>education</th>\n",
       "      <th>Gender</th>\n",
       "      <th>dayofweek</th>\n",
       "      <th>weekend</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-10-07</td>\n",
       "      <td>45</td>\n",
       "      <td>High School or Below</td>\n",
       "      <td>0</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>2</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-10-07</td>\n",
       "      <td>33</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>1</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>3</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>15</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-09-22</td>\n",
       "      <td>27</td>\n",
       "      <td>college</td>\n",
       "      <td>0</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>4</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-09</td>\n",
       "      <td>2016-10-08</td>\n",
       "      <td>28</td>\n",
       "      <td>college</td>\n",
       "      <td>1</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>6</td>\n",
       "      <td>6</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-09</td>\n",
       "      <td>2016-10-08</td>\n",
       "      <td>29</td>\n",
       "      <td>college</td>\n",
       "      <td>0</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Unnamed: 0  Unnamed: 0.1 loan_status  Principal  terms effective_date  \\\n",
       "0           0             0     PAIDOFF       1000     30     2016-09-08   \n",
       "1           2             2     PAIDOFF       1000     30     2016-09-08   \n",
       "2           3             3     PAIDOFF       1000     15     2016-09-08   \n",
       "3           4             4     PAIDOFF       1000     30     2016-09-09   \n",
       "4           6             6     PAIDOFF       1000     30     2016-09-09   \n",
       "\n",
       "    due_date  age             education  Gender  dayofweek  weekend  \n",
       "0 2016-10-07   45  High School or Below       0          3        0  \n",
       "1 2016-10-07   33              Bechalor       1          3        0  \n",
       "2 2016-09-22   27               college       0          3        0  \n",
       "3 2016-10-08   28               college       1          4        1  \n",
       "4 2016-10-08   29               college       0          4        1  "
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df['Gender'].replace(to_replace=['male','female'], value=[0,1],inplace=True)\n",
    "df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "## One Hot Encoding\n",
    "\n",
    "#### How about education?\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "education             loan_status\n",
       "Bechalor              PAIDOFF        0.750000\n",
       "                      COLLECTION     0.250000\n",
       "High School or Below  PAIDOFF        0.741722\n",
       "                      COLLECTION     0.258278\n",
       "Master or Above       COLLECTION     0.500000\n",
       "                      PAIDOFF        0.500000\n",
       "college               PAIDOFF        0.765101\n",
       "                      COLLECTION     0.234899\n",
       "Name: loan_status, dtype: float64"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.groupby(['education'])['loan_status'].value_counts(normalize=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "#### Features before One Hot Encoding\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>age</th>\n",
       "      <th>Gender</th>\n",
       "      <th>education</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>45</td>\n",
       "      <td>0</td>\n",
       "      <td>High School or Below</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>33</td>\n",
       "      <td>1</td>\n",
       "      <td>Bechalor</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1000</td>\n",
       "      <td>15</td>\n",
       "      <td>27</td>\n",
       "      <td>0</td>\n",
       "      <td>college</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>28</td>\n",
       "      <td>1</td>\n",
       "      <td>college</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>29</td>\n",
       "      <td>0</td>\n",
       "      <td>college</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Principal  terms  age  Gender             education\n",
       "0       1000     30   45       0  High School or Below\n",
       "1       1000     30   33       1              Bechalor\n",
       "2       1000     15   27       0               college\n",
       "3       1000     30   28       1               college\n",
       "4       1000     30   29       0               college"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df[['Principal','terms','age','Gender','education']].head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "#### Use one hot encoding technique to conver categorical varables to binary variables and append them to the feature Data Frame\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['Principal', 'terms', 'age', 'Gender', 'weekend', 'Bechalor',\n",
       "       'High School or Below', 'college'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "Feature = df[['Principal','terms','age','Gender','weekend']]\n",
    "Feature = pd.concat([Feature,pd.get_dummies(df['education'])], axis=1)\n",
    "Feature.drop(['Master or Above'], axis = 1,inplace=True)\n",
    "Feature.columns\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Feature Selection\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Let's define feature sets, X:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>age</th>\n",
       "      <th>Gender</th>\n",
       "      <th>weekend</th>\n",
       "      <th>Bechalor</th>\n",
       "      <th>High School or Below</th>\n",
       "      <th>college</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>45</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>33</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>1000</td>\n",
       "      <td>15</td>\n",
       "      <td>27</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>28</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>29</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Principal  terms  age  Gender  weekend  Bechalor  High School or Below  \\\n",
       "0       1000     30   45       0        0         0                     1   \n",
       "1       1000     30   33       1        0         1                     0   \n",
       "2       1000     15   27       0        0         0                     0   \n",
       "3       1000     30   28       1        1         0                     0   \n",
       "4       1000     30   29       0        1         0                     0   \n",
       "\n",
       "   college  \n",
       "0        0  \n",
       "1        0  \n",
       "2        1  \n",
       "3        1  \n",
       "4        1  "
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X = Feature\n",
    "X[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "What are our lables?\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['PAIDOFF', 'PAIDOFF', 'PAIDOFF', 'PAIDOFF', 'PAIDOFF'],\n",
       "      dtype=object)"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "y = df['loan_status'].values\n",
    "y[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "## Normalize Data\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Data Standardization give data zero mean and unit variance (technically should be done after train test split)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([[ 0.51578458,  0.92071769,  2.33152555, -0.42056004, -1.20577805,\n",
       "        -0.38170062,  1.13639374, -0.86968108],\n",
       "       [ 0.51578458,  0.92071769,  0.34170148,  2.37778177, -1.20577805,\n",
       "         2.61985426, -0.87997669, -0.86968108],\n",
       "       [ 0.51578458, -0.95911111, -0.65321055, -0.42056004, -1.20577805,\n",
       "        -0.38170062, -0.87997669,  1.14984679],\n",
       "       [ 0.51578458,  0.92071769, -0.48739188,  2.37778177,  0.82934003,\n",
       "        -0.38170062, -0.87997669,  1.14984679],\n",
       "       [ 0.51578458,  0.92071769, -0.3215732 , -0.42056004,  0.82934003,\n",
       "        -0.38170062, -0.87997669,  1.14984679]])"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X= preprocessing.StandardScaler().fit(X).transform(X)\n",
    "X[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "# Classification\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "Now, it is your turn, use the training set to build an accurate model. Then use the test set to report the accuracy of the model\n",
    "You should use the following algorithm:\n",
    "\n",
    "*   K Nearest Neighbor(KNN)\n",
    "*   Decision Tree\n",
    "*   Support Vector Machine\n",
    "*   Logistic Regression\n",
    "\n",
    "\\__ Notice:\\__\n",
    "\n",
    "*   You can go above and change the pre-processing, feature selection, feature-extraction, and so on, to make a better model.\n",
    "*   You should use either scikit-learn, Scipy or Numpy libraries for developing the classification algorithms.\n",
    "*   You should include the code of the algorithm in the following cells.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Importing all the packages"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.model_selection import train_test_split\n",
    "from sklearn.metrics import f1_score \n",
    "from sklearn.metrics import jaccard_score\n",
    "from sklearn.neighbors import KNeighborsClassifier\n",
    "from sklearn import metrics\n",
    "from sklearn.metrics import classification_report\n",
    "import itertools\n",
    "from sklearn.linear_model import LogisticRegression\n",
    "from sklearn.metrics import log_loss\n",
    "from sklearn.tree import DecisionTreeClassifier\n",
    "import sklearn.tree as tree\n",
    "from sklearn import svm\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# K Nearest Neighbor(KNN)\n",
    "\n",
    "## All data passed to a dataframe at the end of file\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Train set: (276, 8) (276,)\n",
      "Test set: (70, 8) (70,)\n"
     ]
    }
   ],
   "source": [
    "#split up the data\n",
    "X_train, X_test, y_train, y_test = train_test_split( X, y, test_size=0.2, random_state=4)\n",
    "print ('Train set:', X_train.shape,  y_train.shape)\n",
    "print ('Test set:', X_test.shape,  y_test.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([0.67142857, 0.65714286, 0.71428571, 0.68571429, 0.75714286,\n",
       "       0.71428571, 0.78571429, 0.75714286, 0.75714286, 0.67142857,\n",
       "       0.7       , 0.72857143, 0.7       , 0.7       ])"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#useing a for loop to find the best k\n",
    "Ks = 15\n",
    "mean_acc = np.zeros((Ks-1))\n",
    "std_acc = np.zeros((Ks-1))\n",
    "\n",
    "for n in range(1,Ks): \n",
    "    #Train Model and Predict  \n",
    "    neigh = KNeighborsClassifier(n_neighbors = n).fit(X_train,y_train)\n",
    "    yhat=neigh.predict(X_test)\n",
    "    mean_acc[n-1] = metrics.accuracy_score(y_test, yhat)\n",
    "    std_acc[n-1]=np.std(yhat==y_test)/np.sqrt(yhat.shape[0])\n",
    "mean_acc\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAagAAAEYCAYAAAAJeGK1AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjUuMiwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8qNh9FAAAACXBIWXMAAAsTAAALEwEAmpwYAABY1UlEQVR4nO3dd3xb1dnA8d+jPbyzd8IIMyRARlllQ4AwwigBCmWXAim0UAqlUOAtlFJWC6HsDQmUsssqexYyCIEwSiAkcRIyHC9t6d7z/iHLdYwdD+lKV9b5fj60sSzde2RbenTOfc7ziFIKTdM0TbMbR6EHoGmapmkd0QFK0zRNsyUdoDRN0zRb0gFK0zRNsyUdoDRN0zRbchV6AD3Vv39/NXr06EIPQ9M0TcuR+fPnr1dKDWh/e9EFqNGjRzNv3rxCD0PTNE3LERFZ1tHteolP0zRNsyUdoDRN0zRb0gFK0zRNs6WiuwalaZqWL8lkktraWmKxWKGH0if4fD6GDx+O2+3u1v11gNI0TetEbW0t5eXljB49GhEp9HCKmlKKuro6amtrGTNmTLceo5f4NE3TOhGLxejXr58OTjkgIvTr169Hs1EdoDRN0zZBB6fc6enPUgcoTdM0zZZ0gNI0TbO5p556ChHhyy+/LPRQ8koHKE2zWDwVp7axluZ4M6YyCz0crQjNnj2b3XffnTlz5lh6HsMwLD1+T+kApWkWiqfiLG9cTtyIs6p5Fd9s+Ib14fXEU/FCD00rEqFQiPfee4977rlnowBlGAYXXngh48aNY4cdduCWW24BYO7cuey6666MHz+eyZMn09zczP3338+5557b+thp06bx5ptvAlBWVsbll1/OlClT+OCDD7jqqquYNGkS22+/PWeeeSaZrutLlixhv/32Y/z48ey000588803nHjiiTzzzDOtxz3hhBN49tlnc/bcdZq5plkkYSRY0bgCt9ONx+kBwFQmDfEG6qJ1+N1+avw1BNwBHKI/K9rd+S+dz8LvF+b0mBMGT+DmqTdv8j5PP/00U6dOZezYsdTU1LBgwQJ22mkn7rzzTpYuXcrHH3+My+Viw4YNJBIJjj32WB577DEmTZpEU1MTfr9/k8cPh8Nsv/32XHXVVQBsu+22XH755QCceOKJPP/88xx66KGccMIJXHzxxUyfPp1YLIZpmpx++uncdNNNHH744TQ2NvL+++/zwAMP5ORnA3oGpWmWSBpJVjSuwOlwtgYnAIc4CLgDlHvLMZXJqqZVfLvhWz2r0jo1e/ZsZsyYAcCMGTOYPXs2AK+++ipnnXUWLld6nlFTU8NXX33FkCFDmDRpEgAVFRWt3++M0+nkqKOOav36jTfeYMqUKYwbN47XX3+dxYsX09zczMqVK5k+fTqQ3nAbCATYc889WbJkCWvXrmX27NkcddRRXZ6vJ/QMStNyLBOcRASvy9vp/TxODx6nR8+qikRXMx0r1NXV8frrr/PZZ58hIhiGgYhw3XXXoZT6Qdp2R7cBuFwuTPN/1z/b7kXy+Xw4nc7W288++2zmzZvHiBEjuOKKK4jFYq3LfB058cQTeeSRR5gzZw733ntvtk95I/oVoGk5lDJT1DbVolD4XL5uPab9rGpl08rWWVXCSFg8Ys3OnnjiCU466SSWLVvGd999x4oVKxgzZgzvvvsuBxxwALfffjupVAqADRs2sPXWW7Nq1Srmzp0LQHNzM6lUitGjR7Nw4UJM02TFihV89NFHHZ4vE7j69+9PKBTiiSeeANIzseHDh/P0008DEI/HiUQiAJx88sncfPPNAGy33XY5ff46QGlajqTMFLWNtZjKxO/e9Lp/ZzxOD+XecnxuH/WxepbWL2VF4wrCibDOACxBs2fPbl1WyzjqqKN49NFHOf300xk5ciQ77LAD48eP59FHH8Xj8fDYY48xc+ZMxo8fz/77708sFmO33XZjzJgxjBs3jgsvvJCddtqpw/NVVVVxxhlnMG7cOI444ojWpUKAhx56iL/97W/ssMMO7Lrrrnz//fcADBo0iG222YZTTjkl589fNjV1s6OJEycq3bBQsxvDNKhtqiVpJgm4Azk9djwVJ2EkcIqTmkANZZ6yja5radb54osv2GabbQo9DFuLRCKMGzeOBQsWUFlZ2eX9O/qZish8pdTE9vfVMyhNy5JhGqxqXkXKTOU8OAF4Xd7WWVVdpI6l9UupbawlnAhv8tqAplnt1VdfZeutt2bmzJndCk49pZMkNC0LpjJZ1byKeCpOwJP74NSWQxwEPUGgZfNvUy0uh4tqfzXlnnLczu61MNC0XNlvv/1Yvny5ZcfXAUqzHVOZGKZh+zdcU5msbl5NLBVrDRz54nV58bq8mMpkQ2QD68LrCLqDVPur81Lc1CGObieBaFpv6QCl2Uomiy2SjDAwOJBKX6Ut062VUqwJrSGSjOQ9OLXlEEfrzC0zq8oHpRTDKoZR7i3Py/m00qQDlGYbmTf9aDJK0BNkbXgtjbFGBpcPttWndaUU34e+pznRTJmnrNDDaZWZVeVD5rrbaOfovJ1TKz2WfjQVkaki8pWILBGRizv4frWIPCUii0TkIxHZ3srxaPallGJteC1N8SbKvGU4xJH+dC7wXf13rAuvwzALX8hyo3HaKDjlW6ZCxqrmVbb4vWh9k2UBSkScwCzgIGBb4DgR2bbd3X4HLFRK7QCcBPzVqvFo9rY+sp6GWMMPlowy+4IaYg181/Ad4US4QCNMB6fOxlmKvC4vKTPF2vDakskmXNMUy+l/ufLBBx9wxhlnbPI+b7/9NjvttBMul6t1A25PNTQ0cNttt3X6/ZNPPrnXx+6IlTOoycASpdS3SqkEMAc4vN19tgVeA1BKfQmMFpFBFo5Js6EN0Q3URes6nZGICEFPELfTzYrGFaxuXk3KTOV5lFAXqaMu0vk4S1HQE6Qx3khTvKnQQ+nz3nzzTU4++eQOv/fSSy8xderUTT5+5MiR3H///Rx//PG9HkNXASrXrAxQw4AVbb6ubbmtrU+AIwFEZDIwChje/kAicqaIzBOReevWrbNouFohNMYaWRtaS7mnvMvsM5fDRYWvgnAyzNL6pTTFmvL2yb0uUsf66HrKvV2Ps9SUecpYHUpnM2qF8dprr7Hffvtt8j6jR49mhx12wOHY+G3/qaeeYr/99kMpxerVqxk7dizff/89ixcvZvLkyUyYMIEddtiBr7/+mosvvphvvvmGCRMm8Jvf/AalFOeeey7bbrsthxxyCGvXrs3p87IySaKjV3H7d5Nrgb+KyELgU+Bj4AcfjZVSdwJ3QrqSRG6HqRVKc7yZ1c2rKfOW9ehNP+AOYJgGq0OraYw3MqhskKWVFTZEN7Ausq5bQbQUZVLOVzatZFTVKFwOnXuVT+vXr8ftdvd6o+z06dP55z//yaxZs3jppZe48sorGTx4MFdffTXnnXceJ5xwAolEAsMwuPbaa/nss89YuHAhAE8++SRfffUVn376KWvWrGHbbbfl1FNPzdlzs/IvqRYY0ebr4cCqtndQSjUBpwBI+pW/tOU/rY+LJCOsbF5J0BPsVRq50+Gk3FtONBnlu4bvGBAYYElKen20Pj3D0zOnTfI4PUTNKGtCaxhaPlT/rHJoypQpxONxQqEQGzZsYMKECQD8+c9/5sADD+SVV17hgAMOyOoct9xyC9tvvz0/+tGPOO644wDYZZdduPrqq6mtreXII49kyy23/MHj3n77bY477jicTidDhw5ln332yWoc7Vm5xDcX2FJExoiIB5gBbNRqUUSqWr4HcDrwdkvQ0vqwaDJKbVMtAXcAp8OZ1bH8bj8Bd4C14bUsb1ie02Wmxlgja8JrejzDK1V+t59QIkR9rL7QQ+lTPvzwQxYuXMjdd9/NYYcdxsKFC1m4cCEHHnggAC+++GLr9adTTjmFCRMmcPDBB/foHCtXrsThcLBmzZrWthzHH388zz77LH6/nwMPPJDXX3+9w8da+dqwLEAppVLAucDLwBfA40qpxSJyloic1XK3bYDFIvIl6Wy/86waj2YP8VScFY0r8Dq9OVsKsiIlvSnWxKrmVZR5ymy5UdiuyjxlrA2tJZKMFHooJUEpxaJFi1pnVffddx8LFy7khRde6PYxUqkUp5xyCo8++ijbbLMNN954IwDffvstm222Gb/85S857LDDWLRoEeXl5TQ3N7c+9sc//jFz5szBMAxWr17NG2+8kdPnZ+lisVLqBeCFdrfd3ubfHwA/nDdqfVKmBbrH5bGkjJHH6cHtcNMQa6Ap3sTgssG9qvLQHG9mVfMqyr3lOjj1kIgQ8ARY1bSKUVWjbF+uqqcGVWS3YTzTMiVXf1fz589nxx137NYsZu7cuUyfPp36+nqee+45/vCHP7B48WKuueYa9thjD/bYYw8mTJjApEmTOOSQQ3j66ad5+OGHcbvdDB48mMsvv5yamhp22203tt9+ew466CCuu+46Xn/9dcaNG8fYsWPZc889c/K8MnS7DS0vUmaK5Q3LQchLVYiUmSKSiFDpq2RAcEC3Z2uheIja5lqC7mDWy4+lLJqM4nF6GF4xvKiXR3PRbkMp1Vpf0lAGguB0OHE6nFkHqj/+8Y9sscUWrS3hi0FP2m3odBvNcoZpUNuY7jLrd/WukV9PtU1JD9WHGBQc1GWiQ2vihg5OWctcj6qL1NE/2L/QwykIpRSGMjBMo7UVe+bvylAGqVQKhzhwOVw4xNGrQP773/8+18O2FR2gNEtl2lEYyuh1l9lsdDclPZqMsqJxBX63XwenHAm6g6yPrsfn8lHmLZ3NzW1nSwCC/GDvkUMcIOkgljSSIOkPVU5xFvWMM9f0ArtmmbbtKAoRnDIyKekJI8F3Dd9RH63faINvLBVjeeNy/G6/3sOTQyJC0B1kVfMqEkai0MOxlFIK0zRJGAkSqQSGMnCIo8uZkUg6eDlwkDJSxFNxkkay9VpVqdOvRs0Smcrk4WTYNqWB/G4/pjI3qpIOsLxhOT6XTwcnCzgdTlxOF6uaVjGyamSfSzrJXF9KmSlMZaaDkqMXz1H+lzhhKAMjlQ5wmetUpTqr0q/IPqAp1oRCUeYps8XyVKbid2O8kQpvRaGHs5FMSnrCSPBd/Xc4xGFZVqGW5nP5CMVDrAuvY1BZ3yi1mbm+lDJToNjo+lK22i//iUhW16mKmQ5QRUwpRV2kjnWRdTgdTtaG11Ltq6bSV1nQN9y6SB310XpbV/zOpKSbyrRFUO/ryrxlbIhuwO/yU+Gz14eWnmi9vmQa/5v1WBQzRAQRSQcqMwmAU3KT/VcsdIAqUqYyWRdeR0OsgQpvBSKCqUzqY/XUReso95ZT46/Je6O/+mg96yLrWsdkZyKCU3RwypcyTxnfh7/Pa2PFXGi7jLe8YTkxI5azv22/y8+IyhGbvI+IIC1RsHX5z+HAJa7W5cQPPviAe++9l7vuuqvT49x+++3MmjULp9NJWVkZd955J9tu274D0qY1NDTw6KOPcvbZZ3f4/ZNPPplp06Zx9NFH9+i4nSmNMNzHGKbB6uZ0Vlrb1GmHOAh6gq016pY1LGNZwzLCiXBeqn43xZpYE16j69ZpHXI6nLgdblY2ryyKJoeGaWCqdOJD0kjPYOJmnHJvOWWespz8F01Fuz2et958izNPOxOHw4FSioSRIJ6KY5jGRuWOOnP88cfz6aefsnDhQi666CJ+/etf9/hn0pfabWgWSJkpaptqiSajm0w+8Lv9lHvLUShqm2r5tv5bGmONlr0xhOIhXRpI65LX5cU0TVs3OUwYCdaG1/LNhm8wTKM1TdxOH7oy2X8ASSPJq6+9yl777LXJn2lFxf+WVsPhcOvzKdV2G1qOJYwEtU21KKUIeALdeozH6cHj9GCYBmtCa1gr6etUFb6KnLWoyLYyuVZaAp4AjbFGfC4f1f7qQg8HSC/jRVNRNkQ2EE6GcTqcBD1BEpKw7BpTLogIdXV1eNweguVB4qn4JqtUzJo1ixtvvJFEItFa/LVU221oORRLxahtrMXhcPRqT5HT4aTMW4ZSioZ4Q+t1qmpfdVZ7lGKpmN7gqvVYmbeMNeE1+Fy+gu6RM5VJKB5ifXQ9SSOJx+mxXXLP7rvsTjyRbrdRv6GeSTtPAuDqa67mgAMP4NV/v8p+++/XmrCRuU7VkdPOPI3TzjyNObPncOVVV3L3vXcD8Jcb/8LOO+7M5MmTmX70dGLJGDtP2pmrr76apcuWcsQRR7DFllsQT8ZRShFLprsGvP7m6/zk2J9Y1m5DB6giEElGqG2sxePyZD3rEREC7vTsK7NB1ev00j/Yn4A70KMZUKYyud5DpPWUQxz4XX5WNa8qSJPDlJmiOd5MXaSutcpJvhOKuuvdD94F0tegHnrwodagkvHySy9z3vnpRhBnnHYGCxcuZMiQITz7/LM/OFbGjONmcN7M81qXCb9f/T1Oh5N1a9Mdyx0OB8efcDxTfjSFF194kUOnHcrtd9zOmM3GpLMX2+z1Ksp2G1puNMebWd64HJ/bl/OusT6Xr7VNxcqmlSytX0p9tL5b16mSRpLaplpcDpfeQ6T1itvpRpHe0J2v61GZ60vfbviW9ZH1+Nzp10CxfsBSSvHpp58yfsJ4AO665y7mzp/bYXD6+uuvW//9wr9eYIsttwDS7TbOOP0MHnjoAbbeZmtuvulm4H/tNs6deS7TDp3Gp59+Snl5OaHmUOtxdt9jdx5/7PHibLehZac+Ws+a8BrLi5e2vU61LryOdeF1VPvT+6k6CoopM8WKphWISFGlC2v2E3AHaI43Ux+rp8ZfY9l5osko9bF6muPNrdeXevPJ3+9KF8HNlWyLJy+Yv4AJEyZ067n8/ba/8/prr+N2u6muquaee+8B4M9/+jO77bYbu++xO+MnjGfXH+3KQQcfxHPPPMejjz6K2+1m0KBBXPr7S6mpqWGXXXdhx/E7cuCBB3L1tVfz9ptv63YbGaXQbiOzAXd9dH1BsuIyF4wN02i9TuVz+RCRdGXyplpSZqqg1w60vkMpRXO8mZFVI1uXn3N13EgywrrwOuJGHLfT3eNlvA3LNzB267E5G1Ou/enqP7H5Fpvzk2N/UpDzm8rEJS5czu7PdXS7jSLWtlZcuacw+4naX6da1rAMn8tH/2B/6qP1JI1kt7MINa0rmSaHK5tWMrpqdNZLxoZptLb6SJpJvC6v7RIfcuWSSy8p9BAspQOUjRimwfeh7wknw7Z5QflcPnwuH0kjyaqmVTjEoYOTlnMuh4ukJFkdWs3wiuG9WjVIGkkaY43Ux+pbe4/53PZMfNC6Rwcom0iZKVY2rSRpJG1T/bstt9OtkyE0S7VtcjggOKDbj4ulYjREG2iMN+IUJ363P6fL4plmg1r2enpJSQcoG0gYidaOs3p2opWyoDuYzq7LZJh2InOdtC5SRyQZweVwUeYpy3kgcXqc1G+op7qmWgepLCmlqKurw+fr/qxWB6gCy2zAdTqctt2HoWn5IiKUecpY3bwar8v7gyzSthtrE0YCr9Pa60tl/cuoX1/P+nXrLTtHMTMxceDodpaxz+dj+PDh3T6+DlAFFElGWNG4osMXoqaVKqfDidvp3qjJYaE21jpdTioHV1p+nmIVTUap8lXRL9DPkuOXZIBKmanWdsyF0hRrYlVoFQF3oGg3CWqaVbwuL+FEmLXhtTjEQUO0ARHJ+fUlzd5K8p1xTWgNoUQIr9NLmacMv9ufbmCXpySAfG3A1bRiFvQEaYo3tbaR0deASk9JBqiUmWrdeNoYb2RDdAOQTnUNuAMEPcHW6gq5fFEopVgfWZ8u1FqgPU6aVkzsmNGq5U9JBihIX4x1OVwbLa+ZyiSSitAUb2q9j9/lJ+gJ4nOla+H1dsZjhw24WmFEEwamUvjdThwO/XvXtO4q2QDVEYc40hdeW34qSimSZpL1kfWt+fsep6fHy4KGabA6tJpIMmKbDbhafsRTBqF4uhtrNJHC53YS8Lh0oNK0btABahNEpHWpLyNlplqXBRUqXXjSHSToDrZm47WdHdl9A65mHcNUNMeSrV8rIJo0iCUNvC2ByqkDlaZ1SgeoHupoWTCaitIcb0ahWpcFyzxleJyedCsBvQG35CilaIwm6GjjvAJiSYN40sDjchL06kClaR3RASpLm1wWROF29LyCslb8mmJJDHPTZV0U6SXAeMrA63IS8DhxOXUKtaZl6ACVYx0tC2qlJRxPkUiZPXpM20Dl9zhx60ClaTpAaVouxZIGkUSq14/PBCqPy0HA49KBSitpOkBpWo4kDZNQm6SIbCRSJolUArczHag8Lh2otNKjA5Sm5YBpKpqiSXLdnzppmDRGE7icDgIeJ16XrjyilQ4doDQtS0opGmNJzB72uumJlGHSFDVxOQ38bic+tw5UWt+nA5SmZak5liJl9CwpordShkmzYRJJpAh4XDpQaX2aXtjWtCxEEyniKSPv581sAt4QjhNL5v/8mpYPegal2U6xtNhOlzHqfcZeLmQCVSSRwu924Xbm5+fmdEhR/I604qYDlGYrpqloiCbwOB2U+fLT/qQ3Uoa5URmjQjNM1VrzLx8CHhdBr3770Kxl6RKfiEwVka9EZImIXNzB9ytF5DkR+UREFovIKVaOR7O/TAWGaNKgPpLoshpDISilaIolOyxjVCoiiRTJPF1300qXZQFKRJzALOAgYFvgOBHZtt3dzgE+V0qNB/YCbhARXYKhRIViyY3e9FKGSX3EftdYulPGqBQ0x5KtVf41zQpWzqAmA0uUUt8qpRLAHODwdvdRQLmkF7PLgA1AYRf1tYKIJQ2iHQQipdJvhHZ5MwzFkj0uY9RXGaYiXOBrcFrfZmWAGgasaPN1bcttbd0KbAOsAj4FzlNK/eDVLyJnisg8EZm3bt06q8arFUh3KjDEWpb88pXO3dkYOgqipSyaNHTA1ixjZYDqKMWn/UfgA4GFwFBgAnCriFT84EFK3amUmqiUmjhgwIBcj1MroJ5UYDBMRUMkUZAlv1yWMeprQnF7zG61vsfKAFULjGjz9XDSM6W2TgGeVGlLgKXA1haOSbOR3lRgUKSX/Jqi+XtTtKqMUV+hl/o0q1gZoOYCW4rImJbEhxnAs+3usxzYF0BEBgFbAd9aOCbNRkLx3ldgiKfSS35WZ5Llo4xRX6CX+jQrWLaRQSmVEpFzgZcBJ3CvUmqxiJzV8v3bgf8D7heRT0kvCf5WKbXeqjFp9hFNpLJeqjNMRWMkQdDrwu+x5k85n2WMil0onqTa6dEbeLWcsXSnnVLqBeCFdrfd3ubfq4ADrByDZj+JlJmzJSFFeiaWMEzKvW4cOWydnqsyRsVSGSNb6c3CKcptvMFaKy66Fp+WV4apaIolcn49J5EyqY8kcrbMlKsyRo2xBqb/4wBOeHo6H9S+2+eTCWJJoyC1CbW+SQcoLW+UUjRGE5ZVYDBbjh/JMrDkqoyRqUwue+s31DavoC6yjl+8+DPO/NdPWbTm46yPbWehWKrPB2ItP3SA0vKmOZbKSwWGcCJFQySB2Ytz5bKM0QOf3MXby1/ngimX8Oyxr3HRLpexpP6/nPTs0Zz/ys/5esNX2Z/EhkylCl5E146UUjqRpId0gNLyIhLPb1uKpJFe8uvpOXNVxmj+6o+4dd4N7D/mYGZsdxIep5fjtz+Zf814k3Mm/pp5qz/kJ/88hN+98WtWNC3L+nx2o5f6/kcpRTSRYkM4QWM0oVuk9IAU21R84sSJat68eVkdY1nDMkQEl0NXY86HeMqgKVq4Ta7drbwdiiVzUimiLrKeGU8dSsAd5JEjnqLMU/6D+zTGGrh/0Z3M/uwBUmaK6Vv/hNN3PIdBwcFZn98uHCLUBEs3qy8dmNLVRzrapuB0SNE3nYwmo1T5qugX6JfVcURkvlJqYvvb9QxKs5Qd2lJEEqkuK6PnqoyRYRpc8sb5NMebuH6/WR0GJ4BKXxXnTb6I52a8wZHbzOCpLx/nsMf24cYP/0RDrD7rcdiBqRTNsdJb6lNKEYmnqAvHCSdSne6h000nu6YDlGYZO7Wl2FRl9FyWMbp9wV/5aNUHXLL7lWxZs1WX9x8QGMjvdruSp3/yb/bf7GAeWnQP0+bsxZ0LbiGcCOVkTIUUTxkl8+bbPjB19+9eB6rO6QClWcZubSk6qoyeyzJG7614i7s+nsURY4/h8LFH9+ixwytG8se9rucfR73A5GG7ctv8m5n22N489Om9xFPxHIyucMLxVK8SVopFbwNTe20DVTShAxXoAKVZJBxP2TZjqW1l9FyVMfo+tIpL37iAsTVbc/FuV/T6OFvUjOXG/f/OQ4c/ydh+23DDf67msMf34ckvHyNlFudyWV/N6stVYGov0x1ZByodoIpeqiVbzU5/yLGkQSRh7zckw1Q5a9+RNBJc9NovSZpJ/rLfrfhcvqyPOW7geO44+EHuPPghBgYHc9U7v+OoJ6by8jfPY/6wI43t9aWlPqXSxXFzHZjaywSqulA6UBVbQlsu6ABVxFKGSWM0ScowbfOJK1WCbSlu/ug6Fq39mCt+fC2jKsfk9NiTh+3Kg4c9wc0H3IHb4ea3r5/HcU8dzjvL3yi6N6xQPFnUS32m+b/AFLEwMP3gvCozo0qUXKDSAapIZYJT2+Wp9p+48s00W5Ii8n7mwnl16Us88tl9HLfdz9h/s4MsOYeIsNeo/XjsyOe5eu8bCSdCzHz5dE59bgYLVs+15JxWUIqiXOrLBKYNkfwGph+MY6NAVRrVOnSAKkIdBae2Mn/I+Q5UdkuKsNryxu+44q3fMm7gBH495WLLz+d0ODlki8N56ievcOlu/0dt83JOfX4G57x4Cl+uX2z5+XOhmJb6WgNTnmdMXclc0yuFQKU36haZroJTRxyS2RDosGzTZK42uSql+GzdIras2Son13KsEkvFOOmZo1kTXs2cI59jSNnQvI8hmory2OKHuO+TO2iMN7D/mIPZf7ODcIj1nzu3G7BDr5+zCNQEvDmtPJ9LpqmItLSDKYZ3x/Tr24nP7cz7pmirN+oW3zt0CetNcIL/zagiCWsCVa42uQI8/dU/uPKdS9i633bcsN8shlWM6PpBBfDn96/kvxu+4Nap9xQkOAH4XX5OHn8mR21zHA99eg8Pf3ov/176QtcPzIEqbzVPHvMyNf6evzEpBc3xJJV+jwUj671iC0wZmRlVJGEULFBZRc+gikRvg1NHcjmjShomjZHctM/4qu4LTnrmKDav3pLlTctwipM/7XMzuw7fIwdHz53n/vskl731G06b8AtmTrqw0MNp1RRvYk14teXnWR9Zx8yXT+fAzQ7h6r1v6PVxyn1uW5T5KdbA1BkR8Ltd5GOCGjdiDCrrp2dQpSyXwQnaz6h6/4krl5tcmxPNXPjqOVR4q7h16j2EEiEuePVsznnxFM6Z+GtOnXBWXpauurJkw1dc/e5lTBwyhV/sfH6hh7ORCm8FFd4Ky8+zZc1WnDr+59z58a0csuURvf4AEYon8TgdBVvq62uBKUMp8rbNw+otD12+4kVkmogN3hlKVK6DU1vZXGxVSuVsk6tSiivfvphVzbVct+/fqPH3Z2TlaB487AkO3Hwat867gQv+fTahRHPW58pGOBHiwlfPpcxTzrX7/LWoZuC5dtqEsxlduRlXv3sZ0WSkV8fILPXlm2kqQpmKDX0sOPU13Qk8M4CvReQ6EdnG6gFp/2NlcGqrN4GqOZbKySZXgEcX38+rS19i5qQL2XHw/2b5fneAP+19Exf+6Pe8vfx1Tnh6Ot/Uf52Tc/aUUor/e/dSljd9x5/2uZn+gQEFGYddeF1eLtvjalY2r+D2BX/r9XESKTNvmaY6MBWfLgOUUuqnwI7AN8B9IvKBiJwpIh2XadZyIl/Bqa3uBqpoIne9nRat+Zib/nMte43aj5/tcMYPvi8i/HTcKdxxyEM0J5r46dNH8u9vX8zJuXvi8S8e4aVvnuecib9m0tAf5f38drTzkMkcufWxPPzpvVmluYcT1m5P0IGpeHVr6U4p1QT8E5gDDAGmAwtEZKaFYytZhQhObW0qUCVSJuEcbbZsiNVz0Wsz06V89rxuk9fBJg6Zwuzpz7JlzVb85rVzuenDa/NWm27xukVc/8HV7D5iL04Z//O8nLNYnD/5t1T5qrnqnd/1+veRKeKbazowFb/uXIM6VESeAl4H3MBkpdRBwHjAPilMfUShg1Nb7QNVyjBpiuUmY89UJpe+eQF10Tqu3+9WKryVXT5mUHAwd097hGO2OYEHFt3F2S+ezIZoXQ5G07mmeCO/eXUm/QP9+eNe19siUcNOKryV/HbXP/D5+s+Y/dkDvT5O0sjdUp8OTH1Hd15txwA3KaV2UEr9RSm1FkApFQFOtXR0JcZOwamtTKCqjyRytpv+3oW3896Kt/jNLr9n2wHjuv04j9PLpbtfxZV7/pmFa+Zz/FOH89m6RbkZVDumMrnszd+wNrKG6/a9lSpftSXnKXb7jzmIPUfuy6z5N7GyubbXxwnHs1vq04Gp7+lOgPoD8FHmCxHxi8hoAKXUaxaNq+TYNThZYe6qD7ht/k1M3fxQjtnm+F4d4/CxR3P/oY8j4uCUZ4/lqS8fz/Eo4YFFd/HW8te4YMoljBs4PufH7ytEhEt2uwKHOLjm3ct6XXpH0bulPh2Y+q7uBKh/AG3TtYyW27QcKaXgtC6ylotfP59RlWO4fI+rs9oovO2AcTw6/Wl2GjKJK9+5hP9751ISRm6a+81f/RG3zr2B/ccczIztTsrqWB6XA6+r8BtSrTS4bCgzJ17Ae7Vv89I3z/X6OOmlvu5dy9KBqe/rToByKaUSmS9a/m2vGiVFrJSCU8pMcfHr5xNOhvnLvrcScAezPma1r4bbpt7HqRN+wT+/nMOpzx3H96FVWR2zLrKei18/j2HlI/jDj6/JKog6RCj3uqnwu6kKeHA5++41rJ9s+1PGDZzAdR/8Hw2x+l4fJxxPbXKpTwem0tGdV8s6ETks84WIHA6st25IpaOUghPAbfNvZv7qD/n97v/HFjVjc3Zcp8PJLyddyA373ca3DUs47qnDmbvqg14dyzANLnnjfJrijVy/3yzKPNntpij3uVsrJbidDqoDHsp9bpw2LZSaDafDyWW7X01zvImbPry218fpbKlPB6bS050AdRbwOxFZLiIrgN8COtc2S6UWnN5e/gb3Lvw7R259LNO2nG7JOfYdcyCPHPEUVb5qznrhZzy46O4eXw+5Y8Hf+GjVB1yy21WM7bd1VuPxu514XD98ifncTqoDHoIeF32kpmersf225qTxZ/DMf5/go5Xv9/o4bZf6+kpgWrB6LrPm3ch3Dd8WeihFo9vFYkWkrOX+Ba03k4tisUvWL8XldOBxuXM0qp4pteC0qnklM546lCFlw3jwsCfwuryWni+cCHH5Wxfx2ncvc8BmB3PFj6/t1nLi+yve5pyXTuWwsUdx5Z5/zmoMTodQHfB0uTxomopwSz24viKWinHMPw8G4B9HvdDrtikCeN1O4kUclAC+WP8Zt869gfdq3wbAIQ4O2/Iofr7zLwtWCT9XTJVgZPUAy4rFdmtBXEQOAc4GfiUil4vI5VmNpsCaW9o2bwjHCcWS6WKReQoWpRackkaCi16biWkaXL/frZYHJ4Cgp4zr95vFeZMu4tWlL3HiM0exrGHpJh/zfWgVv3vj12xRM5aLd7siq/MLUOFzd+valcMhlPvcVAc8uPvI9Smfy8dlu/+RFU3LuPPjW3t9HAVFXch1acM3/ObVmRzXshXi/Mm/5YUZb3Pcdj/jX0ue4bDH9uW6D/6PDVF9xaQz3dmoeztwLDCT9GvvGGCUxePKC8NURJMGzbEk60PWB6xSC04AN374Jz5b9wlX7nkdIyry92cjIpwy4efcdtD91EXWc8LTR/Dmslc7vG/STHLRa78kaSa5ft9Z+F3+rM4d9Lp7nAzhcjqoCnio8PeN61OTh+3K4WOP5sFP7uK/dV8Wejh5tTq0iiveupijnpjKuyve5Mwdz+X5GW9y8vgzGVo+jN/s8nuePfY1pm15BHMWP8ghc/Zm1rwbaS5wMWQ76nKJT0QWKaV2aPP/ZcCTSqkD8jPEjeViie/jlV9jKtVlNWqX04HbIbhdDjzO7HonlWJweuXbF7jotZn8dPtTuXCXSws2jlXNK7nw1bP5fP1nnLHjOZy103k4Hf9L+77+g6t5+LN7uW7fWzhgs4OzOpfH5chJI75oIkXYRm3Ge6MhVs/0fxzAsPIRPHDYPzb6mfdFG6LruXvh3/nH548iAj/Z5qecOuHn1Pj7d/qY7xq+5bb5N/HKty9Q4a3klPE/Z8Z2J2X9ISlf7LDEF2v5/4iIDAWSwJisRlMkUoZJNGnQFE3PsOojCUKxJPFUz2ZYpRicljUs5cq3L2GHgTty3pSLCjqWoeXDuO/Qxzl87NHc9fEsfvny6TTGGgB4benLPPzZvRy33UlZB6dMSnku+D0uagJe/G4nxTqfqvJVc9Eul/HZuk94/POHCz0cyzTFm7h17g0cMmdvHlv8ENO2PIJnf/IaF+5y6SaDE8Doqs24bt9bmDP9WcYNnMBfP7qOwx7bh8c/f4SkkdjkY0tBd2ZQlwG3APsCs0gvDd+llCrIdah8zqA2RQBnN2ZYpRicoqkoJz1zFOvCa5lz5LMMtsmFYKUU//xyNte+fxWDgoP59ZRL+MNbFzG6anPuO3QObmd2M58Kv9uSDbmGqQjHc1dBPp+UUpz70ql8vGY+Tx79km3+FnIhmooy57MHuW/RHTTFGzlws0P4xc7nM7pqs14fc8Hqudwy93o+XjOP4eUj+cXO5zF180NtO/u0ega1yQDV0qjwR0qp91u+9gI+pVRjVqPJgl0CVHuZgOVxOnA5BY/TgWEqy4PTp2sXcv8nd3LU1jPYZfgeWbdwz4U/vPVbnv3vP5k19V52HfHjQg/nBxat+ZgLXzuXteHvqfBWMmf6cwwtH5bVMf1uJ2U+a7NCEymTUJb16gphZXMtRz0xlclDd+GvB9xpi7/RbCSNBE9+9Th3LbiV9dF17D5iL86d+Gu27r9dTo6vlOK92re4Ze4NfFX3OVtUj+Wcib9mr1H72e5nV9AA1fLAD5RSu2R19hyya4BqT1r+x8qJ05frF3P6v04gnAihUOw8eDLnTrpgo6Z/+fb0V09wxdu/5cwdz+Xsib8q2Di6UhdZzy1zr+eQLQ9n0tDs/ry7m1KeK9GEQSSRKqpZ+UOL7uGGD6/JyXW+QjFMgxe/eZa/z/8rK5tXsNPgST9osplLpjL597cvctv8m1jWuJRxAycwc+IFTB62qyXn6w07BKgrgUWkEyMK/ooolgBltW/qv+b054/H5/Jxx8EP8X7tO9z18a3URdezx4i9OXfSBWzVL78NkP9b9yUnPnMkEwbtzG0H3W/bZYlcEihICSOlFJGEke7Xldcz907KTHHSM0exJvw9Tx3zSrfaq9iFUoo3lv2bWfNu5Jv6r9m633bMnHQBuw7/cV4+lKTMFM/990nuWPA3vg+vZsrQXTl30gWMGzjB8nN3xQ4BqhkIAinSCRMCKKVURVYj6iUdoGBF0zJOfW4GSinuOXQ2oyrTOSvRZITZix/kvk/uoDnRxNTNp/GLnc9v/b6VQolmjn/qCKKpCHOmP0e/wKYvDvcVZV4Xfk/h/o6K6frUl+sXc8LT0zl8q6O5fI9rCj2cbvnPyve4Ze71LF63iNGVm3H2xF+x35ipBekLFk/FeeLLR7n749uoj21g71H7c87EX7FFzVZ5H0tGwQOU3ZR6gPo+tIpTnptBNBnh7mmPdPjH2RRv4sFFd/HwZ/eRNBIcvtXR/HzHmQwqG2LJmJRSXPTaL3n9u5e565BH2GnIJEvOYze5SinPhaRhEoqnm0ra2U0fXssDi+7inmmz2XnI5EIPp1OL1nzMrfNu4KNVHzA4OISzdj6PaVtOt8V7RjgR4tHP7ueBRXcRToY5ZIvDOWvn8xheMTLvYyl4gBKRDq9yK6Xe7sZJpwJ/BZzA3Uqpa9t9/zfACS1fuoBtgAFKqQ2dHbOUA1RdZD2nPj+Dusg67jzk4S4b/dVF1nP3wtt44ovZiMCx257IKeN/To0/uz+m9mZ/9gB//uAqzpt8UU5aopf73MSSBkkbv9k6JH3dyWGzTbX5+sDZHOvdrC2ainL0Ewfhdrh57Mjn81JZpCeWbPiKW+fdyJvLXqXaV8PpO57N0Vsfb7txQnqf2f2f3MnsxQ9gmAbTtz6WM3c6lwGBgXkbgx0CVNvmLj5gMjBfKbVPF49zAv8F9gdqgbnAcUqpzzu5/6HAr7o6bqkGqMZYA6f/63hWNC3n7wfd36MLs6uaV3LHgr/x3NdP4nP5OXHcaZw47tSsK3VDOovwlOdmsOvwPbj5gDuyXvpomw3X3FLVw46sSikvFoapqA/He3X96/3adzj7xZNtlUizomkZt8//Gy8seYagO8jPdjiDE8adkpOWMFZbG17DXR/P4qkvH8PlcDFju5M4ZfzPqfRVWX7uggeoDg40ArhOKXVcF/fbBbhCKXVgy9eXACil/tTJ/R8F3lBK3bWp45ZigAolmvn5Cyfx37ovuWXq3fxo2G69Os639Uu4bf7NvLr0Raq81Zw64Sx+su1Pe13MsyFWz3FPHQYIc6Y/m/ULoqNsuGgiRSjevQZ2+ZKPlPJiEImnq130xqVvXMDL3/6LOdOfzWnrlZ4q5Jt7rrUNsh6nh3JPPtIEFNfsew1n7HxGVkfJZYASYJFSapPrSyJyNDBVKXV6y9cnAlOUUud2cN8A6VnWFh0t74nImcCZACNHjtx52bJlPRpze8UUoKKpKOe+eCqfrFnADfvfxp6j9s36mJ+v+5Rb593A+7XvMDA4mDN3PJfDtzoat6P7b7qmMvnly2fw4cr3ue+wx9h+wA5ZjWlT2XDxlEFzNGmLbLV8p5Tb3YZwvFf7sjZE6zjyHwcyqmoM9x36WN6TDhpi9dz3yR3MWfxg6/LYGTuew8DgoLyOwwpLNnzFk189TjwV6/rOWVKYnL7TyRy05UFZHaezANXlO7SI3AKt7w0OYALwSXfO2cFtnf0lHwq819m1J6XUncCdkJ5BdePcfULCiHPBv3/Bgu/n8qd9bspJcIJ0q/TbDrqfeas/5Ja51/PHd3/PA4vu5Bc7/4qpm0/r1pvFfZ/cwbsr3uTiXa/IOjjBpgusel1OnAEpeEWOnlQpLxVlXjeN0Z6X5Knx9+OCH/2Oy976DU98MZufbHtC1w/KgXAixCOf3ceDi+4ueIKBVbao2YqLdrksL+fKLPFZpTtTiLbraSlgtlLqvW48rhYY0ebr4UBnvbhnALO7ccySkTST/Pa183i/9h2u+PG1TN380JyfY+KQKdx/6OO8s+JNbp17A79741fc/8kdnDPx1/x45D6dvhHPXfUfZs27kambT+PYbX+a9Tg8Lgd+z6av57hautE2xpIFy1QLel19umV7b3hcDrwuZ68SJqZtOZ3nlzzNXz+6jj1H7cug4GALRpgWT8X5xxePcs9C+6Roa13rTpJEEIgppYyWr52AVykV6eJxLtJJEvsCK0knSRyvlFrc7n6VwFJghFIq3NWAS+EalKlMfv/mhbyw5Bku2uUyjt/+5Lyc85Vv/8WseTexomkZOwzckZmTLmTS0B9tdL/1kXUc++Q0yj0VPHLEUwQ9ZVmdt6fZcEqpXmeQZcNOKeV2k03CxIqmZRzzxMHsOuLH3Lj/33M+to43uV7IuIHjc36uUmSHauavAW1rv/uBjhvrtKGUSgHnAi8DXwCPK6UWi8hZInJWm7tOB17pTnAqBUoprn73Ml5Y8gwzJ12Yl+AE6S6fUzc/lCePeZnL9ria78OrOeNfJ/CLF05m8bpFQPrFfvHr5xFOhLh+v1uzDk6QTinvSaq2iFDhdxPM4+ZYEXJWpbwvcjqEQC9/HyMqRvHznX/J69+9wutLX87ZmExl8vI3/+KoJ6Zy5TuX0D8wkDsOfpA7DnlIB6ci0p0Z1EKl1ISubsuXvjyDUkpxw3+u4eHP7uXUCb/gl5MuLNhY4qk4j3/+MPcs/DsN8Xr2HX0g1b4anvhyNlfteR2HjT0q63Nkmw0XSxqEYtYnT5R6Snl39TZhImkmOeGp6dTHNvDkMS9TnsX2B6UU7654k1vn3chXdZ+zefWWnDvxAlsWWu0L7DCDCovITm0OtDMQzWo0Wof+vuCvrb2JZk68oKBj8bq8nLjDaTw/4w3O2uk8/rPyPZ74cjbTt/pJToKTy+nIOlXb53ZSGfDgsPCNx+d26uDUTWW9nGW6HW4u//E11EXX8beP/tLr8y9YPZdTn5vBzJdPJ5Ro5uq9buDxI//F3qP318GpSHVnCnE+8A8RySQ4DCHdAl7Lofs/uZM7F9zCEWOP4Te7XGabF1SZp5yzdv4lx273U95d/iYHbHZI1sfMZMPlgtvC5AmnQyjz2muWbWfZJExsP2AHjtvuZzzy2X0cvMVhPdqI/uX6xdwy7wbeW/EWAwID+d1uVzF9q2Oy7u+lFV639kGJiBvYivR7y5dKqaTVA+tMX1zie+zzh/nTe3/gwM0O4Zq9b+rzVcDLfW587tw+x1wnTxSqSnmxM03Fhl4mTESSYY56Yip+V5A5Rz6Dx7np8kLfNXzLrHk38e+lxdkuvS8o+BKfiJwDBJVSnymlPgXKROTsrEajtXr2v//kT+/9gT1H7ssf976hzwcnr8uZ8+AE/0ue6O3F+vZ0SnnvOLJImAi4g/xut6v4tuFr7v/kzk7vtzq0iivfvoSjnpjKuyve5Mwdz+VfM97ilPE/18Gpj+nOX9IZSqlZmS+UUvUicgZwm3XDKg3//vZFrnj7YqYM3ZXr9r2lR5UcipFDhHKftbPWoNeF0yFZJU+4nY6CttAodgGvi1jK6FXCxB4j92bq5tO46+Pb2H+zgxlTtXnr9zZE13P3wr/zj88fBWDGdidx2oSzqPGXRmuXUtSdV6FDRCTTrLBlH5Re3M3SO8vf4JLXz2eHgTty8wF32LJacq6V56kKg8/txOkQmnpReUIkd9fHSllvK0wA/OZHl/F+7Tv83zuXcve0Rwknw+n2MZ/eR9yIcfjYozlzp5kMKRua41Fbz+V02L4lip10J0C9DDwuIreTLlV0FvCSpaOy0NL6pdQ2LWdo+Yiu72yRuas+4MJXz2HLfltxy9R78LsDBRtLvgQ8Ljyu/C2ZuZ0OqgIeGqOJHn2SL/P2bF+W1rFsEib6Bfrz6ymXcMXbF3Px6+kM0qZ4IwdsdjBn7/wrRldtZsGIreVxOQh60svG8ZRBOJ7q1Qyz1HQnQP2WdKHWX5C+dvwKsMmK43Z23XvXcfv82xlWPpzJQ3dlyrBdmTx0l7wtE3yyZgG/fPlMhpWP5LaD7s9qz0excDkdBAuQDZcp7NoUS5JIdf2p1ee25vpYqSrzukikjF4ttR4+9mie//opXvn2BXYbsSczJ17A1v23y/kYreZ0CEGva6OtCl5XeutCLJkOVIWsL2l3valmvjvpvk7nWDOkTcs2i2/JhiXcPf8hPlz5PvNWf0go0QzAljVbMWXobkwetis7D56UkyoJ7X25fjGn/+sEqn3V3HvoY3ltLFYoIlAd8OIs8KwkFEsS3URvKV2l3BrZtEtpijexOrSSrfptk+NRWc8h6cDU1QcepRTRhEEkkbJFtf6eskU/KBGZABxHev/TUuBJpdQtWY2ol3KZZi4IX9Qt5qOV7/PhyvdYuGY+cSOOS1xsN3AHpgzdlSnDdmOHgROy3lPxbf0STnv+OLxOL/ce+hhDy4dldbxiYUVKeW91VnlCgMqAB7fO2rNEbytMFCMhvZzt9zh79GHHNBXhRMq2DTo7U7AAJSJjSVcZPw6oAx4DLlRKjcpqJFmych9UPBXnkzXz+XDV+3y06gMWr1uEqUx8Lj87DZ7YuiS4Vb9te9S/prZpOac8NwNTGdx76BxGVY7JavzFwutyUuG3V8JBImXSFEvQ9s8+6HER0BtyLZNImb1OmCgmPreToMeV1TXMlGESTqS6tSRtB1YHqE29Kr8E3gEOVUotaTmIPfozW8Tr8jJ52K5MHrYrkF5imL/6Qz5a9T4frvyAmz/6MwCV3iomDd2FKUN3ZfKwXRhZMbrTT0trQqs5818nkjDi3DPt0ZIJTk6H9SnlveFxOagOeFuTJ9xOhw5OFssmYaIYtE2AyJbLma6an0iZhOLJkpl5dmZTr8yjSM+g3hCRl4A5dNyEsM+q8Faw9+j92Xv0/kC6PfTcVR+kZ1gr3+fVpS8CMDg4hCnD0tevJg/dpfXaUl1kPWe+cCKN8QbuPOShkuk9I+Qvpbw3MtebmmOpgiRvlKIyr4uEYdCX8gHSpbDclmSnelwOalzekk+k6G4/qCNIL/XtAzwAPKWUesXy0XXALqWOlFIsb/qOD1e+z0er3mfuqv/QGG8AYLOqLZkybBfmr/6I5Y3f8feDH+hRbbGecDqESr+HWNIgapMLrXrJTOtINgkTdtLdBIhcsXMihS2SJNocpAY4BjhWKbVPViPqJbsEqPZMZfJV3ef8Z+V7fLTyAz7+fi4mJn894C52Gb57zs7TltMhVPn/1+zPMBXheP6b+bWV2X+kaR0p5oSJ3iZA5IodEylsFaDswK4Bqr2EESdlpgi4g5Ycf1Pp20nDJBxPkczzjnURqAl49UZXrVPFmjCRiwSIXLFTIkUhkyS0LHic3i6rMfeWAJV+T6d7izKzmHzvWNdVGLSuFFvCRC4TIHKllBIpdIAqMgKU+93d2rOT2bGeWb+28kKrrsKgdVcxJExYmQCRK6WQSKEDVJEp8/W8/bjf48TndhBJWJNIoRv7aT3hcAhBj8uWCRP5ToDIhXTXZ0f6g2gyZevA31P6XaWIlGXxwpGWF57f7czphdZMd1y7ppRr9uT3uIgme9eSwwoiEHAXLgEiWyJCoOX9IZxIEU/2rgai3egAVSTS2UPZ/7ocDqHc524NVNleaNWN/bTeyqYlR64I4LVRAkS2Mq/voMeVlyW/uMWJGjpAFQGf25nzDaW5uNDqcenGflrvFTphwutyEvA4++QHLIdDcOShrkLStPYc+t3F5rwuJ+UWNtDr7YVWhwjlXnvV2dOKTyESJlzOdGaenRMgtDQdoGzM7XTkrZ5dTy+0lvn6xpJIPhimQSQZIeAO4HQUz8X3fMhnwoTTIQQ8xZUAUer0RwibSpcwym/yQeZCa03Ai8/t7HSBwO929jiTsJSFE2Gq/dVEk1GiyWihh2M7fo/L0n5hIumZWk3Qq4NTkdEByoYyJYwKlU2UudBaFfD8IBBlOoRq3RNLxQh6ggwIDGBM9Rh8Lh9N8SYMszg2quZLmQXLxUL6w1S/oFdfKy1S+rdmMyLptG07LJ+5nA4q/A4SqXTGn2GYOqW8B5RSJI0kw8qHISK4nW6GVQyjOd7MmtAaAAKeQIFHaQ+5TpjwutKJRYXu5KxlRwcoG8mUMLJbVpHH5cDj8pAyTNuNzc7CyTADggPwujYueVXuLcfv9rMuvI7GWCMBT8DSupDFIhcJE26ng6DXpbsj9xH6t2gTPSlhVCg6OHVfwkjgcrio8lV1+H2Xw8WQ8iGMqBxB0kgSToQptsLNuZZJmOgNp0Oo8KeXpe38GtJ6Rv8mbaI3JYw0e1JKEUvFGFI2BIds+iUW9AQZXTWaSl8lzfFmEkbxVfrOpZ4mTDgkXTOvJujVr58+SAcoG8imhJFmP9FklGpfNX63v1v3dzqcDAwOZFTVKExlEkqESno21Z2ECSHdGLMm6MHv0a+dvkoHqALLVQkjzR4y2Xn9A/17/Fi/28/oqtHU+GpoTjQTT8VzPbyikEmY6IiQ3rNXE/QS8Lp0wk4fpwNUAVlRwkgrrHAyzKCyQb3ekOsQB/2D/RlTNQZBaI43Y6rCN6bLtzKvi/axx+NK9zkrt0mWq2Y9HaAKxONyWFrCSMu/aDJKuaecMk9Z1sfyuryMqhrFwOBAIskIsVQsByMsHm0TJjJ1I+2Y4apZqyQ/vnvdTuI5ajfRG26ngwodnPoUU5mkzBQDggNytuwkIlT7qwl6gqwJraEp1kTQEyyZckl+T3oJT1+fLV0lGaCCHme6wZ5ykDRMEoZJMmXmpX9KIUoYadYLJ8IMDA7E4/Tk/Ngep4fhFcNpijexJrQGp8PZ7QSMYqeDU2kryQCV4XI6cDkdZF7qiZTZGrBSRu7X/R1S2BJGmjXiqThep7fTPU+5ICJU+ioJuAOsDa+lKd5E0F06symtNJV0gGovXTHBQRAwTUXCMFuDVrbNv0Sg0q8v7vY1SiniqTijq0fn5YOHLpeklRIdoDrhcAg+h7N1iSFlmL1eDrRrCSMte5FkhH6BfvhcvryeV5dL0kqBpe+YIjJVRL4SkSUicnEn99lLRBaKyGIRecvK8WTD5Ux3j630e+hX5qXS7yHQjV3vxVDCSOudlJnCIQ5q/DUFOb8ul6T1dZZ95BIRJzAL2B+oBeaKyLNKqc/b3KcKuA2YqpRaLiIDrRpPLokIHpeklwO9rk0uB+oSRj0XS8UwTIOgJ1jooWxSJBFhROWIgl8HypRL2hDdwIboBiQPrb4ViqAn2GUpJ03LhpVrApOBJUqpbwFEZA5wOPB5m/scDzyplFoOoJRaa+F4LNPZcqBOke05U5kkjSQV3gqa4k2UecpsmVQSTUap9FXaJog6HU4GBAdQ5atC5SEfNZQIsT6yPid7vjStM1YGqGHAijZf1wJT2t1nLOAWkTeBcuCvSqkH2x9IRM4EzgQYOXKkJYPNpUx2oNZzmXTtKl8VLoeLumgd5Z5yWwUpU5kYptGrckZWczvzs7+uyldFfbQewzQKPoPU+i4r30U7ekdp/9HOBewMHAIcCFwmImN/8CCl7lRKTVRKTRwwYEDuR6rZQjwVx+fyUeWrQkQYEBzAgMAA25X7CSfS5YzyFQzsyCEOBgQGEElGCj0UrQ+zMkDVAiPafD0cWNXBfV5SSoWVUuuBt4HxFo5JsymlFHEjzuCywRvNlvoF+jGobBChRMgWQSqWiuFz+ajwVhR6KAVX7i3H4/SQNJKFHorWR1kZoOYCW4rIGBHxADOAZ9vd5xlgDxFxiUiA9BLgFxaOSbOpcDK9tNe++yxAtb+aoWVDCcVDrdXCC0EpRcJI/CCIlioRYWBwINFUtNBD0fooy65BKaVSInIu8DLgBO5VSi0WkbNavn+7UuoLEXkJWASYwN1Kqc+sGpNmTwkjgdvh3mQlhgpfBSLCyuaVBNyF2fMTSUbo7+/fYRAtVQF3gIA7kK6moX8uWo5Z+ipXSr0AvNDuttvbff0X4C9WjkOzL6UUsWSMUVWjukxZLveWM9IxkhWNK/C5fHm9BpQ0kjjFSU2gMHue7EpEGBAYwLLGZTpAaTmnU820gspUYuhu8dOAO8DIypHEU/G8tkePpqIMLh+s9/10wO/2U+GtIJrUS31abulXm1YwrbOSHlZi8Lv9jKwaSdJI5qXrbCQRodpXTcCta951pn+gP0kzqStZaDmlA5RWMJlZSW/20fhcPkZWjsRUpqXN/AzTQKHoF+hn2Tn6Ao/TQ42/RqedazmlA5RWELmYlXhdXkZWjgSFZctLkWSEQcFBuhBrN9T4a1BK2WI7gNY36ACl5V3KTAHkZFbidroZWTUSpzhz/uk9mowScAco95bn9Lh9lcvhon+wP5GEnkVpuaEDlJZ3kWSEQWW5m5W4HC5GVI7A7XDn7M3RVCaGMhhUNkjveeqBSm8lDnEUdL+a1nfoAKXlVTQZpdxTnvNZidPhZHjFcLwuL6F4KOvjRZIRBgQGWNLCvS/LFK3V16K0XNABSssbwzQwlcnAoDVdVZwOJ8MqhlHmKSOU6H2QymwcrvRV5nB0paPcW47b4W5dytW03tIBSsubcCLMoKC1RVYd4mBI+RAqvBU0x5t7nPac2Tg8uEzveeothzgYEByg90V1IGWm9PJnD+hXoJYX0WSUoCeYl4QDEWFQcBA1/poeB6lIMkJNoKbbG4e1jpV5yvA6vXndTG1nSilCiRApI0UsFdOJJN2kc2c1y5nKJGWmGFE5Im8JByJC/0B/BGF9dH23ekplWrj38+s9T9kSEQaVDWJZw7KSv44XTUZJmanWhpKmMqmL1FEfq8fn8pX8z2dT9AxKs1ymCWG+X4giQv9gfwYGB9IUb+pyf05mz5NuwJcbfrefMk+ZpRup7SxlpmiKN+FxehhTPYYafw0OceByuBhUNohRlaNQStmu35md6AClWaptE8JCqfHXMLR8KM3x5k7X/zPZhWVe3cI8lwYEB5RcvyilFOFEmKSRZFj5MIZXDO/ww5nf7WdU1SgGBgcSToT1NbsO6CU+zTKZJoRjqsYUfC9RpS+9P2dV8yoC7sBGs6RMC3ersgtLmdflpdJbSSgZKolahpkixjX+Gmr8NV3Oxh3ioNpfTZmnjLXhtTTFmgh4CtNOxo70DEqzzKaaEBZCubecYeXDiCQjG6VA6xbu1uoX6IepzD5dSNZUJs3xZhziYFTVKAYEB/RoqdjtdDOsYhgjKkeQNJKE4qE+/fPqLh2gNEskjAQuh6ugS3sdKfOWMbJyJNFktLUaum7hbi23002Nr+8Wko0kI0STUQaXDWZk5Uh8Ll+vjxX0BBldNTqdgZpoLtnrdxk6QPUBmQ2wdpHZSzSkbIgt9xJl1v4TRoK4EdfljPKgyl8FYKu/02wljSRN8SaC7iBjqsdQ6avMyd+R0+Gkf7A/Y6rG4Ha4aYo1lezeKb3QWeQM0yCcDCMILofLFvt3etqEsBAy7ToyMyjNWi6Hi/6B/qyLrKPMU9yJKKYyiSQjuBwuRlaOtOzamtflZXjFcJrjzawNrwXSH65K6cOUDlBFLJMtNLR8KH63n3WRdTTFmvC7/QW7npI0kjjE0eMmhIXgdXltc32sFFT6KtkQ3UDKTBVtEkA0GcVQBv0D/anyVVm+QiAiVPgqCHgC6b1T0Xp87tLZO2W/9Ret20KJEP0D/anwVeB2uhlaPpSRVSMxTINQojAXWaOpKEPKh+i9RNoPOMTBgEBxlkDK7GnyuXyt14jyuXzduneqahQoSmbvlA5QRSqSiFDmKftBT6WAO8Do6tH0D/QnlAjl9c1At0bXulLuLcfj9BTN3qj2e5qGVQwr6Oyl7d6pSCJSlMG+J4pznl3iYqkYLoeLwWWDO1yPziyxlXnKWBdeR1O8iYDb2r0VuWxCqPVdIsLA4EBqm2ptn9bf0z1N+SIirXun7LCsbyU9gyoySSOJaZoMqxjW5QvG4/QwrGIYw8uHW763ItdNCLW+K+gJ4nf7iafihR5KhwzTyGpPU75klvVHVI5IL+v3wb1TOkAVEVOZRFNRhlUM69GnpTJvmaV7K6xqQqj1XQODA21Z6TySjBBLpbdIZLunKV+CniCjq9Ov71Ai1Kf2TumPu0VCKUUoHmrN2OupzN6Kcm85a8NraY43/6DkT28YpqHLBGk9ltkcHUlGbLEdIWEkiKViVPuq6RfoV3QrAQ5x/OD1nY8lv6Rp7bXE4votlLDmRHNrxl42MnsrQokQa0JrUCgC7kCv91Zk0tz74vq3Zq1+gX401TehlCrY3h5TmYQTYdxON6MqR9kiWGaj7es7H5U7gu6gpbNMHaCKQCQRodxTnrMEBBGh3FtOwB1gQ3QDdZG6Xu0JymcTQq3v8Tg9VPurWwuk5lskGcE0TQYGB7YWE+4LMq/vvvC61AHK5rrK2MuG0+FkQHAA5d5y1oTW9GjZz1QmhjJ0mSAtKzX+GhqiDZjKzFuAyCznVXor6R/or2f/NtY3PjL0UT3J2MtGpuzPkLIhre2ou8oGCifCDAgMKJkd7Zo1XA4X/YP989ICPVNx3FRm+u+9fIgOTjanZ1A2lcnYG1U5Ki8vop6UVImn4nidXttVKteKU6W3kg2RDRimYdkHsWgy2prM05eW8/o6/VuyodaMvbLeZexlo6uSKpkmhEPKh+ilPS0nMkvNVlzUz1QcD7gDjKkeQ7W/WgenIqJ/UzaUq4y9bGRKqgwqG0QkEWl98wgnw/T399dFVrWcKveW43a4N2okmY3Mcp6hDL2cV8T0Ep/N5DpjLxsiQpWviqA7yPrIehrjjXicHmoC9q9UrhUXhzgYWDaQlU0rs84+08t5fYcOUDZiZcZeNtxON0PKh7S2F9AveM0KmT01CSPRq+SbpJEkmorq7Lw+RL/T2ETSSGKYhuUZe9nwu/16aU+zTKaQbE9r9JnKJJQI6eW8PkjPoGwg3xl7mmZXfrefMk8Z0WS0WwlCmeW8AcEBejmvD9K/zQIrZMaeptlR/0B/UmZqk3vxkkaydWO5zs7ru/QMqsDadsXVNC1dT67KV0VzovkHzS9NZRJJRnA5XIyoHKGbY/ZxOkAVUGddcTWt1NX4a2iINWxUSFYv55UeS3/DIjJVRL4SkSUicnEH399LRBpFZGHLf5dbOR47sWvGnqbZgdvppp+/H5FkRC/nlTDLZlAi4gRmAfsDtcBcEXlWKfV5u7u+o5SaZtU4Ohkb8VQccUlBMuYyGXvDq4bbNmNP0wqt2l9NQ7wBQxl6Oa9EWbnENxlYopT6FkBE5gCHA+0DVN4NCg6iOd5MU7yJSDKCQxx4nJ68ZNC1zdjThVY1rXNOh5ORlSNxOVx6xlSirAxQw4AVbb6uBaZ0cL9dROQTYBVwoVJqcfs7iMiZwJkAI0eOzHpgmd5H/YP9SRgJoskojfFGmuPNCILL6cLr9OZ86S3brriaVmr0h7jSZmWA6ujdvX3e6AJglFIqJCIHA08DW/7gQUrdCdwJMHHixE33geghj9ODx+mh0leJYRrEUjGa4800J9JFUp0OJx6nJyctoEOJEP0C/XTGnqZpWjdYGaBqgRFtvh5OepbUSinV1ObfL4jIbSLSXym13sJxdcrpcBL0BAl6ggxSg4gbcSLJCI2xRqLJKILgdrrxOD09nl1lMvb6B/pbNHpN07S+xcoANRfYUkTGACuBGcDxbe8gIoOBNUopJSKTSWcV1lk4pm4TEXwuHz6Xjxp/TbrOVzJKc6KZcCIMAk5x4nV5u1wf1xl7mqZpPWdZgFJKpUTkXOBlwAncq5RaLCJntXz/duBo4BcikgKiwAzVVSvXAnE73bidbip8FZjKJJaKEUqE0iX9TQMR6TDRQmfsaZqm9Y7YNB50auLEiWrevHmFHkYrpRQJI0EkGaEp3kTciKOUSgc0h5twMsyoylE6KULTNK0TIjJfKTWx/e26kkSWRKQ1K7DaX03KTLUmWoQSusaepmlab+kAlWMuh4syTxllnrKNyrRomqZpPaN3v1lIBydN07Te0wFK0zRNsyUdoDRN0zRb0gFK0zRNsyUdoDRN0zRb0gFK0zRNsyUdoDRN0zRb0gFK0zRNsyUdoDRN0zRbKrpafCKyDlhW6HF0U3+gIK1DLNTXnpN+Pvamn4/95eI5jVJKDWh/Y9EFqGIiIvM6KoBYzPrac9LPx97087E/K5+TXuLTNE3TbEkHKE3TNM2WdICy1p2FHoAF+tpz0s/H3vTzsT/LnpO+BqVpmqbZkp5BaZqmabakA5SmaZpmSzpAWUBERojIGyLyhYgsFpHzCj2mXBARp4h8LCLPF3os2RKRKhF5QkS+bPk97VLoMWVDRH7V8rf2mYjMFhFfocfUUyJyr4isFZHP2txWIyL/FpGvW/6/upBj7IlOns9fWv7mFonIUyJSVcAh9khHz6fN9y4UESUi/XN5Th2grJECLlBKbQP8CDhHRLYt8Jhy4Tzgi0IPIkf+CryklNoaGE8RPy8RGQb8EpiolNoecAIzCjuqXrkfmNrutouB15RSWwKvtXxdLO7nh8/n38D2SqkdgP8Cl+R7UFm4nx8+H0RkBLA/sDzXJ9QBygJKqdVKqQUt/24m/eY3rLCjyo6IDAcOAe4u9FiyJSIVwI+BewCUUgmlVENBB5U9F+AXERcQAFYVeDw9ppR6G9jQ7ubDgQda/v0AcEQ+x5SNjp6PUuoVpVSq5cv/AMPzPrBe6uT3A3ATcBGQ84w7HaAsJiKjgR2BDws8lGzdTPqP0CzwOHJhM2AdcF/LkuXdIhIs9KB6Sym1Erie9CfY1UCjUuqVwo4qZwYppVZD+oMfMLDA48mlU4EXCz2IbIjIYcBKpdQnVhxfBygLiUgZ8E/gfKVUU6HH01siMg1Yq5SaX+ix5IgL2An4u1JqRyBMcS0dbaTluszhwBhgKBAUkZ8WdlTapojIpaQvBTxS6LH0logEgEuBy606hw5QFhERN+ng9IhS6slCjydLuwGHich3wBxgHxF5uLBDykotUKuUysxqnyAdsIrVfsBSpdQ6pVQSeBLYtcBjypU1IjIEoOX/1xZ4PFkTkZ8B04ATVHFvRN2c9IeiT1reG4YDC0RkcK5OoAOUBURESF/f+EIpdWOhx5MtpdQlSqnhSqnRpC++v66UKtpP6Eqp74EVIrJVy037Ap8XcEjZWg78SEQCLX97+1LESR/tPAv8rOXfPwOeKeBYsiYiU4HfAocppSKFHk82lFKfKqUGKqVGt7w31AI7tby+ckIHKGvsBpxIeqaxsOW/gws9KG0jM4FHRGQRMAG4prDD6b2WmeATwALgU9Kv66IrqSMis4EPgK1EpFZETgOuBfYXka9JZ4pdW8gx9kQnz+dWoBz4d8v7wu0FHWQPdPJ8rD1ncc8wNU3TtL5Kz6A0TdM0W9IBStM0TbMlHaA0TdM0W9IBStM0TbMlHaA0TdM0W9IBSisKLZWSb2jz9YUickWOjn2/iBydi2N1cZ5jWiqnv9Hu9tEtz29mm9tuFZGTuzjeWSJyUhf3OVlEbu3ke6EeDL9XRGRIpvq9iOzVthK+iPxRRF4WEa+IzBGRLa0ej1ZcdIDSikUcODLX5fyzJSLOHtz9NOBspdTeHXxvLXCeiHi6ezCl1O1KqQd7cP6caSlK2x2/Bu7q4PGXkt4veIRSKg78nXStR01rpQOUVixSpDef/qr9N9rPgDIzg5ZP7G+JyOMi8l8RuVZEThCRj0TkUxHZvM1h9hORd1ruN63l8c6W/j1zW/r3/LzNcd8QkUdJb4xtP57jWo7/mYj8ueW2y4HdgdtF5C8dPL91pNtJ/Kz9N0RkcxF5SUTmt4xx65bbrxCRC1v+PalljB+0jLltz56hLY//WkSua3fsG0RkgYi8JiIDWm6bICL/kf/1LKpuuf1NEblGRN4iHUyPaXmOn4jI2x08J4CjgJfanfMC4GDgUKVUtOXmd1p+B90NfFoJ0AFKKyazgBNEpLIHjxlPuo/VONLVPcYqpSaTbhsys839RgN7km4pcrukG/6dRroy+CRgEnCGiIxpuf9k4FKl1EZ9vkRkKPBnYB/SFSomicgRSqmrgHmk66/9ppOxXgtc0MGs7E5gplJqZ+BC4LYOHnsfcJZSahfAaPe9CcCxLT+DYyXdvwcgCCxQSu0EvAX8oeX2B4HftvQs+rTN7QBVSqk9lVI3kC4SeqBSajxwWPsBtfys6ltmSBm7AWcBBymlWpcYlVImsIT070vTAB2gtCLSUhH+QdLN+bprbkt/rjjwDZBpQ/Ep6aCU8bhSylRKfQ18C2wNHACcJCILSbdL6QdkrpN8pJRa2sH5JgFvthRuzVSr/nE3n99S4CPg+Mxtkq6Ivyvwj5Zx3AEMafs4SXdlLVdKvd9y06PtDv2aUqpRKRUjXXNwVMvtJvBYy78fBnZvCf5VSqm3Wm5/oN34H2vz7/eA+0XkDNJNEtsbQnpm2NYSQEj/bNtbS7oau6YB6bYDmlZMbiZdc+6+NrelaPmwJSICtL2O0/bTu9nma5ON//7b1/xSpN9IZyqlXm77DRHZi3SLjo5IF+PvyjWk6+pllswcQINSasImHtPVOdv+DAw6f913p+5Z6/NWSp0lIlNIzzoXisgEpVRdm/tGgfat59cAJwCviUidUqptwoiv5TGaBugZlFZklFIbgMdJL79lfAfs3PLvwwF3Lw59jIg4Wq5LbQZ8BbwM/ELSrVMQkbHSdWPDD4E9RaR/y1LdcaSXz7pFKfUl6VnOtJavm4ClInJMyxhERMa3e0w90CwiP2q5qbvt3h1A5trd8cC7SqlGoF5E9mi5/cTOxi8imyulPlRKXQ6sB0a0u8t/2XiWmhnvf4EjgYdFZEKbb40FFndz7FoJ0DMorRjdAJzb5uu7gGdE5CPSiQadzW425SvSb8SDSF/LiYnI3aTfYBe0zMzW0UXLcaXUahG5BHiD9MzmBaVUT1tEXA183ObrE4C/i8jvSQffOUD7DqanAXeJSBh4E2jsxnnCwHYiMr/l/se23P4z0tfhAqSXO0/p5PF/aUkNF9I/943GpJQKi8g3IrKFUmpJu+/NFZFTgGdFZG8gBEQz3XM1DXQ1c03rE0SkLJN0ICIXA0OUUucVeFiIyHRgZ6XU77u436+AJqXUPfkZmVYM9AxK0/qGQ1pmbi5gGXByYYeTppR6SkT6deOuDcBDFg9HKzJ6BqVpmqbZkk6S0DRN02xJByhN0zTNlnSA0jRN02xJByhN0zTNlnSA0jRN02zp/wEWDmhD9CQrdQAAAABJRU5ErkJggg==",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "#plot the Ks\n",
    "plt.plot(range(1,Ks),mean_acc,'g')\n",
    "plt.fill_between(range(1,Ks),mean_acc - 1 * std_acc,mean_acc + 1 * std_acc, alpha=0.10)\n",
    "plt.fill_between(range(1,Ks),mean_acc - 3 * std_acc,mean_acc + 3 * std_acc, alpha=0.10,color=\"green\")\n",
    "plt.legend(('Accuracy ', '+/- 1xstd','+/- 3xstd'))\n",
    "plt.ylabel('Accuracy ')\n",
    "plt.xlabel('Number of Neighbors (K)')\n",
    "plt.tight_layout()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['PAIDOFF', 'PAIDOFF', 'PAIDOFF', 'PAIDOFF', 'PAIDOFF'],\n",
       "      dtype=object)"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#train the model with the best K (7) \n",
    "neigh = KNeighborsClassifier(n_neighbors = 7).fit(X_train,y_train)\n",
    "yhat=neigh.predict(X_test)\n",
    "yhat[:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Decision Tree\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['COLLECTION', 'COLLECTION', 'PAIDOFF', 'PAIDOFF', 'PAIDOFF'],\n",
       "      dtype=object)"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#load up decision tree and create a prediction\n",
    "Decision_Tree = DecisionTreeClassifier(criterion=\"entropy\", max_depth = 4)\n",
    "Decision_Tree.fit(X_train,y_train)\n",
    "predTree = Decision_Tree.predict(X_test)\n",
    "predTree[0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Support Vector Machine\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['COLLECTION', 'PAIDOFF', 'PAIDOFF', 'PAIDOFF', 'PAIDOFF'],\n",
       "      dtype=object)"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#load SVM\n",
    "clf = svm.SVC(kernel='rbf')\n",
    "clf.fit(X_train, y_train) \n",
    "svmYhat = clf.predict(X_test)\n",
    "svmYhat [0:5]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Logistic Regression\n",
    "\n",
    "## All data passed to a dataframe at the end of file\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [],
   "source": [
    "LR = LogisticRegression(C=0.01, solver='liblinear').fit(X_train,y_train)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "['COLLECTION' 'PAIDOFF' 'PAIDOFF' 'PAIDOFF' 'PAIDOFF']\n",
      "[[0.5034238  0.4965762 ]\n",
      " [0.45206111 0.54793889]\n",
      " [0.30814132 0.69185868]\n",
      " [0.34259428 0.65740572]\n",
      " [0.32025894 0.67974106]]\n"
     ]
    }
   ],
   "source": [
    "LRyhat = LR.predict(X_test)\n",
    "LRyhat_prob = LR.predict_proba(X_test)\n",
    "print(LRyhat[:5])\n",
    "print(LRyhat_prob[:5])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Model Evaluation using Test set\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "First, download and load the test set:\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "--2022-06-06 20:06:31--  https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/ML0101ENv3/labs/loan_test.csv\n",
      "Resolving s3-api.us-geo.objectstorage.softlayer.net (s3-api.us-geo.objectstorage.softlayer.net)... 67.228.254.196\n",
      "Connecting to s3-api.us-geo.objectstorage.softlayer.net (s3-api.us-geo.objectstorage.softlayer.net)|67.228.254.196|:443... connected.\n",
      "HTTP request sent, awaiting response... 200 OK\n",
      "Length: 3642 (3.6K) [text/csv]\n",
      "Saving to: ‘loan_test.csv’\n",
      "\n",
      "loan_test.csv       100%[===================>]   3.56K  --.-KB/s    in 0s      \n",
      "\n",
      "2022-06-06 20:06:31 (100 MB/s) - ‘loan_test.csv’ saved [3642/3642]\n",
      "\n"
     ]
    }
   ],
   "source": [
    "!wget -O loan_test.csv https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/ML0101ENv3/labs/loan_test.csv"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "### Load Test set for evaluation\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Unnamed: 0</th>\n",
       "      <th>Unnamed: 0.1</th>\n",
       "      <th>loan_status</th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>effective_date</th>\n",
       "      <th>due_date</th>\n",
       "      <th>age</th>\n",
       "      <th>education</th>\n",
       "      <th>Gender</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>9/8/2016</td>\n",
       "      <td>10/7/2016</td>\n",
       "      <td>50</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>female</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>5</td>\n",
       "      <td>5</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>300</td>\n",
       "      <td>7</td>\n",
       "      <td>9/9/2016</td>\n",
       "      <td>9/15/2016</td>\n",
       "      <td>35</td>\n",
       "      <td>Master or Above</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>21</td>\n",
       "      <td>21</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>9/10/2016</td>\n",
       "      <td>10/9/2016</td>\n",
       "      <td>43</td>\n",
       "      <td>High School or Below</td>\n",
       "      <td>female</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>24</td>\n",
       "      <td>24</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>9/10/2016</td>\n",
       "      <td>10/9/2016</td>\n",
       "      <td>26</td>\n",
       "      <td>college</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>35</td>\n",
       "      <td>35</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>800</td>\n",
       "      <td>15</td>\n",
       "      <td>9/11/2016</td>\n",
       "      <td>9/25/2016</td>\n",
       "      <td>29</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>male</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Unnamed: 0  Unnamed: 0.1 loan_status  Principal  terms effective_date  \\\n",
       "0           1             1     PAIDOFF       1000     30       9/8/2016   \n",
       "1           5             5     PAIDOFF        300      7       9/9/2016   \n",
       "2          21            21     PAIDOFF       1000     30      9/10/2016   \n",
       "3          24            24     PAIDOFF       1000     30      9/10/2016   \n",
       "4          35            35     PAIDOFF        800     15      9/11/2016   \n",
       "\n",
       "    due_date  age             education  Gender  \n",
       "0  10/7/2016   50              Bechalor  female  \n",
       "1  9/15/2016   35       Master or Above    male  \n",
       "2  10/9/2016   43  High School or Below  female  \n",
       "3  10/9/2016   26               college    male  \n",
       "4  9/25/2016   29              Bechalor    male  "
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "test_df = pd.read_csv('loan_test.csv')\n",
    "test_df.head()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Process Data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Unnamed: 0</th>\n",
       "      <th>Unnamed: 0.1</th>\n",
       "      <th>loan_status</th>\n",
       "      <th>Principal</th>\n",
       "      <th>terms</th>\n",
       "      <th>effective_date</th>\n",
       "      <th>due_date</th>\n",
       "      <th>age</th>\n",
       "      <th>education</th>\n",
       "      <th>Gender</th>\n",
       "      <th>dayofweek</th>\n",
       "      <th>weekend</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-08</td>\n",
       "      <td>2016-10-07</td>\n",
       "      <td>50</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>1</td>\n",
       "      <td>3</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>5</td>\n",
       "      <td>5</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>300</td>\n",
       "      <td>7</td>\n",
       "      <td>2016-09-09</td>\n",
       "      <td>2016-09-15</td>\n",
       "      <td>35</td>\n",
       "      <td>Master or Above</td>\n",
       "      <td>0</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>21</td>\n",
       "      <td>21</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-10</td>\n",
       "      <td>2016-10-09</td>\n",
       "      <td>43</td>\n",
       "      <td>High School or Below</td>\n",
       "      <td>1</td>\n",
       "      <td>5</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>24</td>\n",
       "      <td>24</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>1000</td>\n",
       "      <td>30</td>\n",
       "      <td>2016-09-10</td>\n",
       "      <td>2016-10-09</td>\n",
       "      <td>26</td>\n",
       "      <td>college</td>\n",
       "      <td>0</td>\n",
       "      <td>5</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>35</td>\n",
       "      <td>35</td>\n",
       "      <td>PAIDOFF</td>\n",
       "      <td>800</td>\n",
       "      <td>15</td>\n",
       "      <td>2016-09-11</td>\n",
       "      <td>2016-09-25</td>\n",
       "      <td>29</td>\n",
       "      <td>Bechalor</td>\n",
       "      <td>0</td>\n",
       "      <td>6</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Unnamed: 0  Unnamed: 0.1 loan_status  Principal  terms effective_date  \\\n",
       "0           1             1     PAIDOFF       1000     30     2016-09-08   \n",
       "1           5             5     PAIDOFF        300      7     2016-09-09   \n",
       "2          21            21     PAIDOFF       1000     30     2016-09-10   \n",
       "3          24            24     PAIDOFF       1000     30     2016-09-10   \n",
       "4          35            35     PAIDOFF        800     15     2016-09-11   \n",
       "\n",
       "    due_date  age             education  Gender  dayofweek  weekend  \n",
       "0 2016-10-07   50              Bechalor       1          3        0  \n",
       "1 2016-09-15   35       Master or Above       0          4        1  \n",
       "2 2016-10-09   43  High School or Below       1          5        1  \n",
       "3 2016-10-09   26               college       0          5        1  \n",
       "4 2016-09-25   29              Bechalor       0          6        1  "
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "test_df['Gender'].replace(to_replace=['male','female'], value=[0,1],inplace=True)\n",
    "test_df['due_date'] = pd.to_datetime(test_df['due_date'])\n",
    "test_df['effective_date'] = pd.to_datetime(test_df['effective_date'])\n",
    "test_df['dayofweek'] = test_df['effective_date'].dt.dayofweek\n",
    "test_df['weekend'] = test_df['dayofweek'].apply(lambda x: 1 if (x>3)  else 0)\n",
    "test_df.head()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Load Data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [],
   "source": [
    "test_Feature = test_df[['Principal','terms','age','Gender','weekend']]\n",
    "test_Feature = pd.concat([test_Feature,pd.get_dummies(df['education'])], axis=1)\n",
    "test_Feature.drop(['Master or Above'], axis = 1,inplace=True)\n",
    "X = test_Feature\n",
    "X = X.dropna()\n",
    "X= preprocessing.StandardScaler().fit(X).transform(X)\n",
    "y = test_df['loan_status'].values\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Make Predictions"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [],
   "source": [
    "yhat=neigh.predict(X)\n",
    "predTree = Decision_Tree.predict(X)\n",
    "svmYhat = clf.predict(X)\n",
    "LRyhat = LR.predict(X)\n",
    "LRyhat_prob = LR.predict_proba(X)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Evaluation of all predictions\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Algorithm</th>\n",
       "      <th>Jaccard</th>\n",
       "      <th>F1-score</th>\n",
       "      <th>LogLoss</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Knn</td>\n",
       "      <td>0.740000</td>\n",
       "      <td>0.728821</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Decision tree</td>\n",
       "      <td>0.659091</td>\n",
       "      <td>0.736682</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>SVM</td>\n",
       "      <td>0.745098</td>\n",
       "      <td>0.714414</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>LogisticRegression</td>\n",
       "      <td>0.716981</td>\n",
       "      <td>0.649142</td>\n",
       "      <td>0.570205</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "            Algorithm   Jaccard  F1-score   LogLoss\n",
       "0                 Knn  0.740000  0.728821       NaN\n",
       "1       Decision tree  0.659091  0.736682       NaN\n",
       "2                 SVM  0.745098  0.714414       NaN\n",
       "3  LogisticRegression  0.716981  0.649142  0.570205"
      ]
     },
     "execution_count": 34,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#load scores into data frame\n",
    "scores = []\n",
    "scores.append([\"Knn\",jaccard_score(y, yhat,pos_label='PAIDOFF'),f1_score(y, yhat, average='weighted')])\n",
    "scores.append([\"Decision tree\",jaccard_score(y, predTree,pos_label='PAIDOFF'),f1_score(y, predTree, average='weighted')])\n",
    "scores.append([\"SVM\",jaccard_score(y, svmYhat,pos_label='PAIDOFF'),f1_score(y, svmYhat, average='weighted')])\n",
    "scores.append([\"LogisticRegression\",jaccard_score(y, LRyhat,pos_label='PAIDOFF'),f1_score(y, LRyhat, average='weighted'),log_loss(y, LRyhat_prob)])\n",
    "df = pd.DataFrame(scores, columns =['Algorithm', 'Jaccard', 'F1-score','LogLoss'])\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Report\n",
    "\n",
    "You should be able to report the accuracy of the built model using different evaluation metrics:\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "| Algorithm          | Jaccard | F1-score | LogLoss |\n",
    "| ------------------ | ------- | -------- | ------- |\n",
    "| KNN                | ?       | ?        | NA      |\n",
    "| Decision Tree      | ?       | ?        | NA      |\n",
    "| SVM                | ?       | ?        | NA      |\n",
    "| LogisticRegression | ?       | ?        | ?       |\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "button": false,
    "new_sheet": false,
    "run_control": {
     "read_only": false
    }
   },
   "source": [
    "<h2>Want to learn more?</h2>\n",
    "\n",
    "IBM SPSS Modeler is a comprehensive analytics platform that has many machine learning algorithms. It has been designed to bring predictive intelligence to decisions made by individuals, by groups, by systems – by your enterprise as a whole. A free trial is available through this course, available here: <a href=\"http://cocl.us/ML0101EN-SPSSModeler?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork20718538-2022-01-01\">SPSS Modeler</a>\n",
    "\n",
    "Also, you can use Watson Studio to run these notebooks faster with bigger datasets. Watson Studio is IBM's leading cloud solution for data scientists, built by data scientists. With Jupyter notebooks, RStudio, Apache Spark and popular libraries pre-packaged in the cloud, Watson Studio enables data scientists to collaborate on their projects without having to install anything. Join the fast-growing community of Watson Studio users today with a free account at <a href=\"https://cocl.us/ML0101EN_DSX?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork20718538-2022-01-01\">Watson Studio</a>\n",
    "\n",
    "<h3>Thanks for completing this lesson!</h3>\n",
    "\n",
    "<h4>Author:  <a href=\"https://ca.linkedin.com/in/saeedaghabozorgi?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork20718538-2022-01-01?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkML0101ENSkillsNetwork20718538-2022-01-01\">Saeed Aghabozorgi</a></h4>\n",
    "<p><a href=\"https://ca.linkedin.com/in/saeedaghabozorgi\">Saeed Aghabozorgi</a>, PhD is a Data Scientist in IBM with a track record of developing enterprise level applications that substantially increases clients’ ability to turn data into actionable knowledge. He is a researcher in data mining field and expert in developing advanced analytic methods like machine learning and statistical modelling on large datasets.</p>\n",
    "\n",
    "<hr>\n",
    "\n",
    "## Change Log\n",
    "\n",
    "| Date (YYYY-MM-DD) | Version | Changed By    | Change Description                                                             |\n",
    "| ----------------- | ------- | ------------- | ------------------------------------------------------------------------------ |\n",
    "| 2020-10-27        | 2.1     | Lakshmi Holla | Made changes in import statement due to updates in version of  sklearn library |\n",
    "| 2020-08-27        | 2.0     | Malika Singla | Added lab to GitLab                                                            |\n",
    "\n",
    "<hr>\n",
    "\n",
    "## <h3 align=\"center\"> © IBM Corporation 2020. All rights reserved. <h3/>\n",
    "\n",
    "<p>\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python",
   "language": "python",
   "name": "conda-env-python-py"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.12"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
