---
title: Prepare Data for classifying Iris tutorial in Machine Learning services (preview) | Microsoft Docs
description: This full-length tutorial shows how to use Azure Machine Learning services (preview) end-to-end. This is part 1 on data preparation.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/28/2017
---

# Classifying Iris part 1: Prepare data
Azure Machine Learning services (preview) is an integrated, end-to-end data science and advanced analytics solution for professional data scientists to prepare data, develop experiments and deploy models at cloud scale.

This tutorial is part one of a three part series. In this tutorial, let's walk through the basics of Azure Machine Learning services (preview). In this tutorial, you learn how to:
> [!div class="checklist"]
> * Create a project in the Azure Machine Learning Workbench
> * Create a data preparation package
> * Generate Python/PySpark code to invoke data prep package

This tutorial uses the timeless [Iris flower dataset](https://en.wikipedia.org/wiki/Iris_flower_data_set) to keep things simple. The screenshots are Windows-specific, but the macOS experience is almost identical.

## Prerequisites
- Create an Azure Machine Learning Experimentation Account
- Install Azure Machine Learning Workbench

You can follow the [Install and create Quickstart](quickstart-installation.md) to install the Azure Machine Learning Workbench application. That installation also includes the command-line interface (CLI).

## Create a new project in Azure ML Workbench
1. Launch the Azure Machine Learning Workbench app, and log in if needed. In the **PROJECTS** pane, click on the **+** icon to create a **New Project**.

   ![new workspace](media/tutorial-classifying-iris/new_ws.png)

2. Fill in the **Create New Project** details. 

   ![New Project](media/tutorial-classifying-iris/new_project.png)

   - Fill in the **Project name** field with a name for the project. For example, use the value **myIris**.
   - Choose the **Project directory** in which the project is created. For example, use the value **C:\Temp**. 
   - Enter the **Project description**. 
   - The **Git repository** field is optional and can be left blank. You can provide an existing empty Git repo (a repo with no master branch) on VSTS. Doing so allows you to enable roaming and sharing scenarios later. For more information, please reference the [Using Git repo](using-git-ml-project.md) article. 
   - Choose a **Workspace**, for example this tutorial uses **IrisGarden**. 
   - Select the **Classifying Iris** template from the project template list. 

3. Click on the **Create** button to create the project. The project is now created and opened for you.

## Create a data preparation package
1. Open the **iris.csv** file from the **File View**. The file is a simple table with 5 columns and 150 rows. It has four numerical feature columns and a string target column. It does not have column headers.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > It is not recommended to include data files in your project folder, particularly when the file size is large. We include **iris.csv** in this template for demonstration purposes because it is tiny. For more information, please reference the [How to read and write large data files](how-to-read-write-files.md) article.

2. In the **Data View**, click on the **+** icon to add a new data source. The **Add Data Source** wizard launches. 

   ![data view](media/tutorial-classifying-iris/data_view.png)

3. Complete the Data Preparation Wizard. 
   - On the first screen, select the **File(s)/Directory** option, then click **Next**.
   - On the second screen, choose the **iris.csv** local file, for example "C:\Temp\myIris\iris.csv".
   - On the third screen **File Details**, accept default values.
   - On the fourth screen **Data Types**, change the **DATA TYPE** from _String_ to _Numeric_ for _Column1_ through _Column4_ since these columns are numeric values. 
   - On the fifth and the sixth screens, accept the default settings.
   - Click the **Finish** button.

   ![select iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Make sure you select the **iris.csv** file from within the current project directory for this exercise, otherwise latter steps may fail. 

4. A new file **iris-1.dsource** is created. The file is named uniquely with a dash-one since the sample project already comes with an unnumbered **iris.dsource** file.  

   The file opens and the data is shown. A series of column headers, from **Column1** to **Column5**, are automatically added to this dataset. Scroll to the bottom and notice the last row of the dataset is empty. It is because of an extra line break in the csv file.

   ![iris data view](media/tutorial-classifying-iris/iris_data_view.png)

5. Now click on the **Metrics** button. Observe the histograms and a complete set of statistics that are calculated for you for each column. You can also click the **Data** button to see the data again. 

   ![iris data view](media/tutorial-classifying-iris/iris_metrics_view.png)

6. Click on the **Prepare** button. The **Prepare** dialog pops up. 

   The sample project comes with a **iris.dprep** file, so by default it asks you to create a new data flow in that existing **iris.dprep** data prep package. 

   Change the dropdown value to **+New Data Preparation Package**, and enter a new value "iris-1", then click on **OK**.

   ![Iris data view](media/tutorial-classifying-iris/new_dprep.png)

   A new data prep package named **iris-1.dprep** is created and opened in data preparation editor.

7. Now let's do some basic data preparation. Rename the column names by clicking on each column header and make the header text editable. 

   Enter **Sepal Length**, **Sepal Width**, **Petal Length**, **Petal Width**, and **Species** for the five columns respectively.

   ![Rename columns](media/tutorial-classifying-iris/rename_column.png)

8. To count distinct values, select the **Species** column, and right-click on it. Choose **Value Counts**. 

   ![Click Value Counts](media/tutorial-classifying-iris/value_count.png)

   This action opens the **Inspectors** pane, and displays   a histogram with four bars. Notice the target column has three distinct values: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa**, and a **(null)** value.

9. Filter out nulls by selecting the bar from the graph representing the null value. There is one row with a **(null)** value. To get rid of this row, click on the **-** filter button.

   ![Value count histogram](media/tutorial-classifying-iris/filter_out.png)

10. Notice the individual steps detailed in the **STEPS** pane. As you worked on column renaming and filtering null value rows, each action was recorded as a data prep step. You can edit individual steps to adjust the settings, reorder steps, and even remove steps.

   ![Steps](media/tutorial-classifying-iris/steps.png)

11. Now close the data prep editor by clicking the **X** on the tab named **iris-1** with the graph icon. Your work is automatically saved into the **iris-1.dprep** file shown under **Data Preparations** heading.

## Generate Python/PySpark code to invoke data prep package

1. Right-click on the **iris-1.dprep** file to bring up context menu, and choose **Generate Data Access Code File**. 

   ![generate code](media/tutorial-classifying-iris/generate_code.png)

2. A new file named **iris-1.py** opens with following  lines of code:

   ```python
   # This code snippet will load the referenced package and return a DataFrame.
   # If the code is run in a PySpark environment, the code will return a
   # Spark DataFrame. If not, the code will return a Pandas DataFrame.

   from azureml.dataprep.package import run
   df = run('iris-1.dprep', dataflow_idx=0)
   ```

   This code snippet invokes the logic you created as a data preparation package. Depending on the context in which this code is run, `df` represents different kinds of data frames. A [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) is used when executed in Python runtime, or a [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) is used when executed in a Spark context. 

   For more information on how to prepare data in Azure Machine Learning Workbench, reference the [Getting Started with Data Preparation](data-prep-getting-started.md) guide.

## Next steps
In this first part of the three part tutorial series, you have used the Azure Machine Learning Workbench to:
> [!div class="checklist"]
> * Create a new project 
> * Create a data preparation package
> * Generate Python/PySpark code to invoke data prep package

You are ready to move on to the next part in the series, to build a machine learning model.
> [!div class="nextstepaction"]
> [Build a model](tutorial-classifying-iris-part-2.md)
