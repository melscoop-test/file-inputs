# file-inputs

How to load input parameters from external JSON and YAML files into your workflow.

You can read environment variables from a file in a GitHub Actions workflow using a combination of shell commands and GitHub Actions syntax. 

### Here's how you can do it for JSON.

If you have a JSON file, for example `config.json` with the following content:

```
{
  "parameter1": "value1",
  "parameter2": "value2"
}
```

You could then read these parameters and set environment variables within your workflow as outlined in [the following YML file](https://github.com/melscoop-test/file-inputs/blob/master/.github/workflows/inputs-json.yml):

```
name: Inputs from JSON test

on:
  workflow_dispatch:

jobs:
  your_job:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Install jq (if not already installed)
      run: |
        sudo apt update
        sudo apt install jq -y

    - name: Load parameters from JSON file
      id: load_params
      run: |
        echo "PARAM1=$(jq -r '.parameter1' config.json)" >> $GITHUB_ENV
        echo "PARAM2=$(jq -r '.parameter2' config.json)" >> $GITHUB_ENV

    - name: Use the parameters
      run: |
        echo "Parameter 1: ${{ env.PARAM1 }}"
        echo "Parameter 2: ${{ env.PARAM2 }}"
```

### Here's how you can do it for YAML.

If you have a YAML/YML file, for example `config.yml` with the following content:

```
parameter1: value1
parameter2: value2
```

To read the parameters and set environment variables in your workflow, you can use the following steps:

```
jobs:
  your_job:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Load parameters from YAML file
      id: load_params
      run: |
        echo "PARAM1=$(cat config.yml | grep parameter1 | cut -d ' ' -f 2)" >> $GITHUB_ENV
        echo "PARAM2=$(cat config.yml | grep parameter2 | cut -d ' ' -f 2)" >> $GITHUB_ENV

    - name: Use the parameters
      run: |
        echo "Parameter 1: ${{ env.PARAM1 }}"
        echo "Parameter 2: ${{ env.PARAM2 }}"
```






