# Better Safe than Sorry: The Impact of Copying&Pasting Vulnerable Code from Stack Overflow
GitHub repository for Master's Thesis.

## Summary
[- Introduction](#introduction) <br/>
[- Replication](#replication)

## Introduction
Online programming discussion platforms such as **Stack Overflow** serve as a rich source of information for software developers, providing benefits. However, when it comes to code **security** there are some caveats to bear in mind: Due to the complex nature of code security, it is very difficult to provide ready-to-use and secure solutions for every problem. Developers often reuse Stack Overflow code in their **GitHub projects**. So, locating vulnerable statements in source code is crucial to assure software security because vulnerable code can flow easily across software repositories. High false positive rate is a big obstacle for traditional static analysis. In this work, we empirically study the presence of the Common Weakness Enumeration – CWE, in code snippets of C/C++ Stack Overflow posts through two approaches: **static analysis** and **machine learning**. We want to detect the line code where there is a vulnerability because it should be easier for developers to find possible bugs, static
analysis already allows it, for machine learning a novel approach based on ensemble learning is used. Moreover, we explore the **evolution** of reused vulnerable code snippets in GitHub repositories and assess if a Stack Overflow code snippet is still flawed from one commit to another. We find that the static analysis-based approach is better in detecting vulnerabilities (with a precision of 100% on both classes, a recall of 65% for the negative class, and a recall of 57% for the positive class) compared to
the machine learning approach especially when the codes under consideration come from real-world projects, rather than codes in synthetic datasets containing intentional flaws. Indeed, although we obtain good performances with machine learning, the model cannot discriminate classes. To investigate the propagation of Stack Overflow code snippets in GitHub, we use two approaches: a query on the table   ```PostReferenceGH``` of dataset SOTorrent and a clone detection tool. For vulnerable code snippets, we build the history and show that in a total of 18 repositories analyzed, 11 of these repositories have never removed vulnerable code from their projects.

## Replication
To replicate our study, first of all you will need Python 3 and Jupyter Notebook. Below are specified the order in which the notebooks have to be executed with details, if necessary, on how to run.

### Vulnerability detection
- **Create_datasets_repositories_codeSnippets.ipynb**: to run the query on SOTorrent is necessary to download json file with credentials from Google BigQuery:
  - Login with your Google account on [Google Cloud](https://console.cloud.google.com/home/dashboard) and select a project if exists, otherwise create a new project.
  - Menu > IAM & Admin > Service Accounts.
  - Select your service account.
  - Click Keys > Add key > Create new key.
  - Select JSON, then click Create. Your new public/private key pair is generated and downloaded to your machine as a new file.
  - Add to the user enviroment variables a variable called GOOGLE_APPLICATION_CREDENTIALS having as value the path to the JSON file downloaded before.
- **1-Static analysis-based approach.ipynb**: 
  - Download CppCheck, the version used in this thesis is available [here](https://github.com/danmar/cppcheck/releases/download/2.9/cppcheck-2.9-x64-Setup.msi).
- **2.1-Preparation dataset Juliet.ipynb**: 
  - Download Juliet dataset, available [here](https://samate.nist.gov/SARD/test-suites/112).
  - Download Joern and add . Please make sure Gradle is installed. Then:
       ```sh
    git clone https://github.com/octopus-platform/joern
    cd joern
    ./build.sh
    ```
    For Windows operating systems could be useful to add in the joern folder this [file](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/main/1-Vulnerability_Detection/joern/joern-parse.bat), that is the equivalent .bat file for joern-parse.
- **2.2-VELVET pre-training.ipynb**
- **2.3-Preparation dataset D2A.ipynb**:
  - Download D2A dataset, available [here](https://drive.google.com/drive/folders/1Q-yApGmz-HyNdrgN8jxy2ugG-cmmGu7B?usp=sharing).
- **2.4-VELVET fine-tuning.ipynb**
- **2.5-Machine Learning-based approach.ipynb**:
  - To save result after ensemble learning update two files: [VELVET/src/data_loader.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/dbde82c7ea71bc16927cf17d2e3d356c872a84c2/1-Vulnerability_Detection/VELVET/src/data_loader.py) and [VELVET/src/run_model.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/dbde82c7ea71bc16927cf17d2e3d356c872a84c2/1-Vulnerability_Detection/VELVET/src/run_model.py).
- **3-Comparison between approaches.ipynb**:
  - Download ReVeal dataset, available [here](https://drive.google.com/drive/folders/1KuIYgFcvWUXheDhT--cBALsfy1I4utOy).
  - For testing dataset code snippets update two files: [VELVET/src/meta_model.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/490adfdfc3e2af6f1e6b97dba73abd23626e4276/1-Vulnerability_Detection/VELVET/src/meta_model.py) and [VELVET/src/run_model.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/490adfdfc3e2af6f1e6b97dba73abd23626e4276/1-Vulnerability_Detection/VELVET/src/run_model.py).
  
### Vulnerability propagation
