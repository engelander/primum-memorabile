# primum-memorabile
primum memorabile
# This workflow will upload a Python Package 
# This workflow will upload Ansible


# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.

name: Upload Python Package

on:
  release:
    types: [published]

jobs:
  deploy:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.x'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install build       
    - name: Install dependencies python3.8
      run: |
        sudo add-apt-repository ppa:deadsnakes/ppa      
        sudo apt update
        sudo apt install python3.8
        sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
    - name: Install dependencies ansible
      run: |     
        sudo apt-get install software-properties-common
        sudo apt-add-repository ppa:ansible/a
        sudo apt-get install ansible
        ansible --version
    - name: Build package
      run: python -m build      
    - name: Publish package
      uses: pypa/gh-action-pypi-publish@27b31702a0e7fc50959f5ad993c78deac1bdfc29
      with:
        user: __token__
        password: ${{ secrets.PYPI_API_TOKEN }}
