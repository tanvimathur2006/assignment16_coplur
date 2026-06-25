# Dockerized PySpark RDD Application with JupyterLab

This project implements a PySpark RDD processing pipeline within an interactive JupyterLab container environment. It analyzes employee data to sort entries by compensation tiers, groups payroll metrics by department, and isolates top performers.

## Project Structure
```text
├── Dockerfile
├── README.md
├── employee_processing.ipynb
└── employees.csv
```

## Setup & Execution Instructions

### Step 1: Build the Docker Image
Execute the build command from your root directory:
```bash
docker build -t assign16-app .
```

### Step 2: Launch JupyterLab Container
To interactively run and modify the notebook, run the container while exposing port `8080`. We map the local directory to the workspace container folder so files you create/save sync back to your machine.

**On Linux/macOS:**
```bash
docker run --rm -it -p 8080:8080 -v $(pwd):/workspace assign16-app
```

**On Windows (PowerShell):**
```bash
docker run --rm -it -p 8080:8080 -v ${PWD}:/workspace assign16-app
```

### Step 3: Accessing the Application
1. Open your web browser and navigate to: `http://localhost:8080`
2. Open the file browser on the left side panel and click on `employee_processing.ipynb`.
3. Click **Run > Run All Cells** from the top application toolbar to execute the PySpark script.
4. Output results will generate directly below the execution cells, and the `top_3_highest_paid` folder containing partition files will save directly into your root directory structure.

### Step 4: How to Retrieve and View the Output
Because Apache Spark processes data in parallel across multiple threads, the final task saves its output inside a directory (`top_3_highest_paid/`) split across multiple files called **partitions** (`part-00000`, `part-00001`, etc.).

To safely consolidate and view your top 3 highest-paid employees list from your local computer's terminal, use the following commands:

**On Linux / macOS:**
```bash
# Concatenate and view all partition data segments at once
cat top_3_highest_paid/part-*
```

**On Windows (PowerShell):**
```bash
# Display content from all matching partition chunks
Get-Content top_3_highest_paid\part-*
```

#### Expected Consolidated File Content:
```text
4,Priya,Finance,70000.0
3,Neha,IT,65000.0
7,Rohit,Finance,60000.0
```
