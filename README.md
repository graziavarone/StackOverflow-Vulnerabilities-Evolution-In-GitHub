# Better Safe than Sorry: Investigating the Evolution of Vulnerable Code Snippets Copied from Stack Overflow
GitHub repository for Master's Thesis.

## Summary
[- Introduction](#introduction) <br/>
[- Replication](#replication)

## Introduction
Online programming discussion platforms such as **Stack Overflow** serve as a rich source of information for software developers, providing benefits. However, when it comes to code **security** there are some caveats to bear in mind: Due to the complex nature of code security, it is very difficult to provide ready-to-use and secure solutions for every problem. Developers often reuse Stack Overflow code in their **GitHub projects**. So, locating vulnerable statements in source code is crucial to assure software security because vulnerable code can flow easily across software repositories. High false positive rate is a big obstacle for traditional static analysis. In this work, we empirically study the presence of the Common Weakness Enumeration – CWE, in code snippets of C/C++ Stack Overflow posts through two approaches: **static analysis** and **machine learning**. We want to detect the line code where there is a vulnerability because it should be easier for developers to find possible bugs, static
analysis already allows it, for machine learning a novel approach based on ensemble learning is used. Moreover, we explore the **evolution** of reused vulnerable code snippets in GitHub repositories and assess if a Stack Overflow code snippet is still flawed from one commit to another. We find that the static analysis-based approach is better in detecting vulnerabilities (with a precision of 100% on both classes, a recall of 65% for the negative class, and a recall of 57% for the positive class) compared to
the machine learning approach especially when the codes under consideration come from real-world projects, rather than codes in synthetic datasets containing intentional flaws. Indeed, although we obtain good performances with machine learning, the model cannot discriminate classes. To investigate the propagation of Stack Overflow code snippets in GitHub, we use two approaches: a query on the table   ```PostReferenceGH``` of dataset SOTorrent and a clone detection tool. For vulnerable code snippets, we build the history and show that in a total of 18 repositories analyzed, 11 of these repositories have never removed vulnerable code from their projects.

## Replication
To replicate our study, first of all, you will need Python 3 and Jupyter Notebook. Below is specified the order in which the notebooks have to be executed with details, if necessary, on how to run.

### Vulnerability detection
- **Create_datasets_repositories_codeSnippets.ipynb**: to run the query on SOTorrent is necessary to download the JSON file with credentials from Google BigQuery:
  - Log in with your Google account on [Google Cloud](https://console.cloud.google.com/home/dashboard) and select a project if exists, otherwise create a new project.
  - Menu > IAM & Admin > Service Accounts.
  - Select your service account.
  - Click Keys > Add key > Create new key.
  - Select JSON, then click Create. Your new public/private key pair is generated and downloaded to your machine as a new file.
  - Add to the user environment variables a variable called GOOGLE_APPLICATION_CREDENTIALS having as value the path to the JSON file downloaded before.
- **1-Static analysis-based approach.ipynb**: 
  - Download CppCheck, the version used in this thesis is available [here](https://github.com/danmar/cppcheck/releases/download/2.9/cppcheck-2.9-x64-Setup.msi).
- **2.1-Preparation dataset Juliet.ipynb**: 
  - Download the Juliet dataset, available [here](https://samate.nist.gov/SARD/test-suites/112).
  - Please make sure Gradle is installed. Then download Joern:
       ```sh
    git clone https://github.com/octopus-platform/joern
    cd joern
    ./build.sh
    ```
    For Windows operating systems could be useful to add in the joern folder this [file](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/main/1-Vulnerability_Detection/joern/joern-parse.bat), which is the equivalent .bat file for joern-parse.
- **2.2-VELVET pre-training.ipynb**
- **2.3-Preparation dataset D2A.ipynb**:
  - Download the D2A dataset, available [here](https://drive.google.com/drive/folders/1Q-yApGmz-HyNdrgN8jxy2ugG-cmmGu7B?usp=sharing).
- **2.4-VELVET fine-tuning.ipynb**
- **2.5-Machine Learning-based approach.ipynb**:
  - To save the result after ensemble learning you can update two files: [VELVET/src/data_loader.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/dbde82c7ea71bc16927cf17d2e3d356c872a84c2/1-Vulnerability_Detection/VELVET/src/data_loader.py) and [VELVET/src/run_model.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/dbde82c7ea71bc16927cf17d2e3d356c872a84c2/1-Vulnerability_Detection/VELVET/src/run_model.py).
- **3-Comparison between approaches.ipynb**:
  - Download the ReVeal dataset, available [here](https://drive.google.com/drive/folders/1KuIYgFcvWUXheDhT--cBALsfy1I4utOy).
  - For testing, dataset code snippets update two files: [VELVET/src/meta_model.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/490adfdfc3e2af6f1e6b97dba73abd23626e4276/1-Vulnerability_Detection/VELVET/src/meta_model.py) and [VELVET/src/run_model.py](https://raw.githubusercontent.com/graziavarone/StackOverflow_Vulnerabilities/490adfdfc3e2af6f1e6b97dba73abd23626e4276/1-Vulnerability_Detection/VELVET/src/run_model.py).
  
### Vulnerability propagation
  - **1-Repositories to analyze with SOTorrent.ipynb**
  - **2.1-Repositories to analyze with clone detection (C).ipynb**:
    - If you are on a Windows system, install [cygwin](https://www.cygwin.com/install.html).
    - NiCad requires that FreeTXL be installed on your system. FreeTXL can be downloaded from http://www.txl.ca/.
    - Clone this [repository](https://github.com/bumper-app/nicad) and add the content of this [folder](https://github.com/graziavarone/StackOverflow_Vulnerabilities/tree/main/2-Vulnerability_Propagation/cpp_txl) in the txl folder of the repository cloned.
    - Use the command ```make``` in the nicad directory to precompile all NiCad tools and TXL programs before installing NiCad.  
    - To install NiCad for use by everyone on your system, copy the entire contents of the nicad directory to any place where it can reside permanently. On most systems, the appropriate place would be /usr/local/lib/nicad.
    - Edit the command scripts "nicad6" and "nicad6cross" in the nicad directory to specify the directory where you installed NiCad. For example, on a system where you put NiCad       in /usr/local/lib/nicad, you would modify the scripts to say: LIB=/usr/local/lib/nicad
    - Copy the edited "nicad6" and "nicad6cross" scripts to any "bin" directory which is on the command search path of the intended users of NiCad.  On most systems, the          appropriate place  would be /usr/local/bin.
  - **2.2-Repositories to analyze with clone detection (C++).ipynb**
  - **3.1-Building GHCodeSnippetsHistory (C).ipynb**
  - **3.2-Building GHCodeSnippetsHistory (C++).ipynb**
  - **3.3-Building GHCodeSnippetsHistory from SOTorrent.ipynb**
  - **4-Assessing fixes of vulnerabilities.ipynb**
  - **5-Quantitative Analysis.ipynb**
  - **6-Qualitative Analysis.ipynb**
