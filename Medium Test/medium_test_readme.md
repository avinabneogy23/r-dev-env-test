##  Medium Test

#### Building R for a specific Revision number

First, I verified the current R version in the container by launching R in the terminal after following the steps in [Building R](https://contributor.r-project.org/r-dev-env/tutorials/building_r/) and using 

```bash

svn checkout -r 86123 https://svn.r-project.org/R/trunk/ $TOP_SRCDIR

#to checkout the specific revision 86123
```
Screenshot for R revision number 86123 

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/medium_0.png)

#### bash script to build R from SVN Revision number

Next we have the script that takes a revision number as an argument and performs all the necessary steps to build R from that specific SVN revision:

```bash
#!/bin/bash

# Check if revision number is provided
if [ -z "$1" ]; then
    echo "Please provide a revision number."
    exit 1
fi

# Set revision number
REVISION=$1

# Run svn cleanup to clean up any existing SVN locks or issues in the source directory ($TOP_SRCDIR)
svn cleanup $TOP_SRCDIR

# Checkout the desired revision
svn update -r $REVISION $TOP_SRCDIR

# Download recommended packages
$TOP_SRCDIR/tools/rsync-recommended

# Create and change to build directory
mkdir -p $BUILDDIR
cd $BUILDDIR

# Configure and build R
$TOP_SRCDIR/configure --with-valgrind-instrumentation=1
make

# Check R
make check

echo "R built successfully for revision $REVISION."

```
Screenshot showing the bash script 

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/medium_1.png)

#### **How It Functions**

1. The script is executed with a specific SVN revision as an argument:
Example:

```bash
./build_r.sh 86123
```
Sequential screenshots from running the script for SVN revision number 86122

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/medium_2.png)

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/medium_3.png)

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/medium_4.png)