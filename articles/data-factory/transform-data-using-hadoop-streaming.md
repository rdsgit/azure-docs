---
title: Transform data using Hadoop Streaming activity
description: Learn how to use Hadoop Streaming Activity in Azure Data Factory or Synapse Analytics pipelines to transform data by running Hadoop Streaming programs on a Hadoop cluster. 
titleSuffix: Azure Data Factory & Azure Synapse
author: nabhishek
ms.author: abnarain
ms.service: data-factory
ms.subservice: tutorials
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
---

# Transform data using Hadoop Streaming activity in Azure Data Factory or Synapse Analytics
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1](v1/data-factory-hadoop-streaming-activity.md)
> * [Current version](transform-data-using-hadoop-streaming.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

The HDInsight Streaming Activity in an Azure Data Factory or Synapse Analytics [pipeline](concepts-pipelines-activities.md) executes Hadoop Streaming programs on [your own](compute-linked-services.md#azure-hdinsight-linked-service) or [on-demand](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight cluster. This article builds on the [data transformation activities](transform-data.md) article, which presents a general overview of data transformation and the supported transformation activities.

To learn more, read through the introduction articles to [Azure Data Factory](introduction.md) and [Synapse Analytics](../synapse-analytics/overview-what-is.md) and do the [Tutorial: transform data](tutorial-transform-data-spark-powershell.md) before reading this article. 

## JSON sample
```json
{
    "name": "Streaming Activity",
    "description": "Description",
    "type": "HDInsightStreaming",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mapper": "MyMapper.exe",
        "reducer": "MyReducer.exe",
        "combiner": "MyCombiner.exe",
        "fileLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "filePaths": [
            "<containername>/example/apps/MyMapper.exe",
            "<containername>/example/apps/MyReducer.exe",
            "<containername>/example/apps/MyCombiner.exe"
        ],
        "input": "wasb://<containername>@<accountname>.blob.core.windows.net/example/input/MapperInput.txt",
        "output": "wasb://<containername>@<accountname>.blob.core.windows.net/example/output/ReducerOutput.txt",
        "commandEnvironment": [
            "CmdEnvVarName=CmdEnvVarValue"
        ],
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```

## Syntax details

| Property          | Description                              | Required |
| ----------------- | ---------------------------------------- | -------- |
| name              | Name of the activity                     | Yes      |
| description       | Text describing what the activity is used for | No       |
| type              | For Hadoop Streaming Activity, the activity type is HDInsightStreaming | Yes      |
| linkedServiceName | Reference to the HDInsight cluster registered as a linked service. To learn about this linked service, see [Compute linked services](compute-linked-services.md) article. | Yes      |
| mapper            | Specifies the name of the mapper executable | Yes      |
| reducer           | Specifies the name of the reducer executable | Yes      |
| combiner          | Specifies the name of the combiner executable | No       |
| fileLinkedService | Reference to an Azure Storage Linked Service used to store the Mapper, Combiner, and Reducer programs to be executed. Only **[Azure Blob Storage](./connector-azure-blob-storage.md)** and **[ADLS Gen2](./connector-azure-data-lake-storage.md)** linked services are supported here. If you don't specify this Linked Service, the Azure Storage Linked Service defined in the HDInsight Linked Service is used. | No       |
| filePath          | Provide an array of path to the Mapper, Combiner, and Reducer programs stored in the Azure Storage referred by fileLinkedService. The path is case-sensitive. | Yes      |
| input             | Specifies the WASB path to the input file for the Mapper. | Yes      |
| output            | Specifies the WASB path to the output file for the Reducer. | Yes      |
| getDebugInfo      | Specifies when the log files are copied to the Azure Storage used by HDInsight cluster (or) specified by scriptLinkedService. Allowed values: None, Always, or Failure. Default value: None. | No       |
| arguments         | Specifies an array of arguments for a Hadoop job. The arguments are passed as command-line arguments to each task. | No       |
| defines           | Specify parameters as key/value pairs for referencing within the Hive script. | No       | 

## Next steps
See the following articles that explain how to transform data in other ways: 

* [U-SQL activity](transform-data-using-data-lake-analytics.md)
* [Hive activity](transform-data-using-hadoop-hive.md)
* [Pig activity](transform-data-using-hadoop-pig.md)
* [MapReduce activity](transform-data-using-hadoop-map-reduce.md)
* [Spark activity](transform-data-using-spark.md)
* [.NET custom activity](transform-data-using-dotnet-custom-activity.md)
* [Stored procedure activity](transform-data-using-stored-procedure.md)
