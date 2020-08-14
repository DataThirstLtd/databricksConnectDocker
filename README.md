## Databricks-Connect Container

This container is designed for developing PySpark application in VS Code using Databricks-Connect. Technically it should work for Scala, Java & R - though I haven't tried.

It can be used as a Docker Container or as a cloud hosted container using  [CodeSpaces](https://visualstudio.microsoft.com/services/visual-studio-codespaces/).

## Requirements

### Mac OS
* VS Code with the [Remote Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
* Docker or a [CodeSpace](https://visualstudio.microsoft.com/services/visual-studio-codespaces/) Plan

### Windows 10
* VS Code with the [Remote Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
* Docker or a [CodeSpace](https://visualstudio.microsoft.com/services/visual-studio-codespaces/) Plan
* If using Docker:
  * WSL or [WSL2](https://devblogs.microsoft.com/commandline/announcing-wsl-2/) (WSL2 Preferred)

### Alternative
For a more advanced option you can use any OS and a [Remote Docker Host](https://code.visualstudio.com/docs/remote/containers-advanced#_developing-inside-a-container-on-a-remote-docker-host).

This maybe useful if you cannot run local containers and do not want to use a cloud container. 

## Getting Started

* Open an empty folder in VS Code
* Create a directory called .devcontainer
* Create an empty file in the directory called devcontainer.json
* Paste this code into the file:

```
{
	"context": "..",
	"image": "datathirstltd/dbconnect:7.1.0",

	"settings": {
		"python.pythonPath": "/opt/conda/envs/dbconnect/bin/python",
		"python.venvPath": "/opt/conda/envs/dbconnect/lib/python3.7/site-packages/pyspark/jars"
	},

	//  Optional command - could add your own environment.yml file here (you must keep --name the same)
	// "postCreateCommand": "conda env update --file environment.yml --name dbconnect",
	
	// Rather than storing/committing your bearer token here we recommend using a local variable and passing thru "DATABRICKS_API_TOKEN": "${localEnv:DatabricksToken}",
	// You can manually set these as environment variables if you prefer
	"containerEnv": {
		"DATABRICKS_ADDRESS": "https://westeurope.azuredatabricks.net/",
		"DATABRICKS_API_TOKEN": "dapia12345678901234567890",
		"DATABRICKS_CLUSTER_ID": "0000-11111-hello123",
		"DATABRICKS_ORG_ID": "1234567890",
		"DATABRICKS_PORT": "8787"
	},
	"extensions": [
		"ms-python.python"
	]
}  
```

**IMPORTANT: Correct the image tag to the version of Databricks Runtime your cluster is running. Currently we only support Databricks 6+**

* Update the Databricks Variables for your environment
* Optionally add any additional extensions you want to the extensions block.

**IMPORTANT: Changing any setting in the devcontain.json after the container has been build requires you to rebuild the container for it take effect**

To open using Docker locally:
* Click on the Green icon in the bottom left of VSCode and select "Reopen in Container"

To open in a CodeSpace:
* Commit your folder to a repo first
* Open the Remote Explorer (left hand toolbar)
* Ensure CodeSpaces is selected in the top drop down
* Click + (Create new CodeSpace)
* Follow the prompts

The first pull can be a little slow as the image is quite big. But once it is cached rebuilding the container should take just a few seconds.

## Test it out

First from command prompt check that you databricks connect install can connect:

```
databricks-connect test
```

If create a test.py file and paste this code:
```
from pyspark.sql import SparkSession
spark = SparkSession\
.builder\
.getOrCreate()

print("Testing simple count")

# The Spark code will execute on the Azure Databricks cluster.
print(spark.range(100).count())
```

Press F5 and select the Python debugger.

### Why?
Because setting up Databricks-Connect (particulary on Windows is a PIA). This allow:
* A common setup between team members
* Multiple side by side versions
* Ability to reset your environment
* Even run the whole thing from a [browser](https://docs.microsoft.com/en-gb/visualstudio/online/how-to/browser)!

## GitHub Repository

### Issues & Contributions
https://github.com/DataThirstLtd/databricksConnectDocker

## Docker Hub
https://hub.docker.com/r/datathirstltd/dbconnect

## More Information
About Data Thirst - https://datathirst.net
About VSCode and Containers: https://code.visualstudio.com/docs/remote/containers