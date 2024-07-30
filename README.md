# Creating & Documenting Python Modules with Sphinx

*This guide will walk through the process of setting up Sphinx for auto-documenting Python modules. It will review how to create a sample Python file, configure Sphinx, generate HTML documentation, and update the documentation.*

---

## Walkthrough

1. **Create Initial Project Directory**

   First, create irectory for Sphinx project and navigate into it:

   ```bash
   mkdir my_sphinx_project
   cd my_sphinx_project
   ```

2. **Create Sample Python File**

   ```bash
   touch example_module.py
   ```

3. **Add Content to `example_module.py`**

   ```python
   """
   This module demonstrates a generic function with in-line comments
   for Sphinx auto documentation.

   Functions:
       example_function(var1, var2, var3, var4): Performs an example operation.
   """

   def example_function(var1: int, var2: str, var3: float, var4: list) -> bool:
       """
       This function performs an example operation to demonstrate Sphinx auto documentation.
       
       Parameters:
       var1 (int): The first parameter, an integer value.
       var2 (str): The second parameter, a string value.
       var3 (float): The third parameter, a floating-point value.
       var4 (list): The fourth parameter, a list of values.
       
       The function processes the input parameters and performs some operations.

       Returns:
       bool: True if the operation is successful, False otherwise.
       """
       # Check if var1 is positive
       if var1 <= 0:
           return False
       
       # Ensure var2 is not empty
       if not var2:
           return False
       
       # Perform a sample calculation using var3
       result = var3 * 2
       
       # Verify the length of var4
       if len(var4) < 3:
           return False
       
       # Final check before returning True
       return True
   ```

4. **Install Sphinx documentation library using pip**

   ```bash
   pip install -U sphinx
   ```

5. **Initialize Sphinx**

   Run `sphinx-quickstart` command and follow prompts. Choose separate source and build directories:

   ```bash
   sphinx-quickstart
   ```

   ```
   > Separate source and build directories (y/n) [n]: y
   > Project name: sphinxdemo
   > Author name(s): nicholas.piesco
   > Project release: 0.1
   ```

   This will create necessary Sphinx directory structure.

6. **Edit `conf.py`**

   Edit `conf.py` file located in `source` directory. Add path to your module and configure project information:

   ```python
   # Configuration file for the Sphinx documentation builder.
   #
   # For the full list of built-in configuration values, see the documentation:
   # https://www.sphinx-doc.org/en/master/usage/configuration.html

   import os
   import sys

   # Add path to module
   sys.path.insert(0, os.path.abspath('.'))

   # -- Project information -----------------------------------------------------
   # https://www.sphinx-doc.org/en/master/usage/configuration.html#project-information

   project = 'sphinxdemo'
   copyright = '2024, nick piesco'
   author = 'nick piesco'
   release = '20240625'

   # -- General configuration ---------------------------------------------------
   # https://www.sphinx-doc.org/en/master/usage/configuration.html#general-configuration

   extensions = [
       'sphinx.ext.autodoc', 
       'sphinx.ext.napoleon'
   ]
   # Add autodoc extension and napoleon for numpy & google in line comments

   templates_path = ['_templates']
   exclude_patterns = []

   # -- Options for HTML output -------------------------------------------------
   # https://www.sphinx-doc.org/en/master/usage/configuration.html#options-for-html-output

   html_theme = 'alabaster'
   html_static_path = ['_static']
   ```

7. **Move `example_module.py` to Source Directory**

   ```bash
   mv example_module.py source/
   ```

8. **Edit `index.rst`**

   Edit `index.rst` in `source` directory to include module in `toctree`:

   ```rst
   .. sphinxdemo documentation master file, created by
      sphinx-quickstart on Tue Jun 25 12:52:40 2024.
      You can adapt this file completely to your liking, but it should at least
      contain the root `toctree` directive.

   Welcome to sphinxdemo's documentation!
   ======================================

   .. toctree::
      :maxdepth: 2
      :caption: Contents:

      example_module

   Indices and tables
   ==================

    :ref:`genindex`
    :ref:`modindex`
    :ref:`search`
   ```

9. **Create `example_module.rst`**

   Create file named `example_module.rst` in `source` directory with following content:

   ```rst
   Example Module
   ==============

   .. automodule:: example_module
      :members:
      :undoc-members:
      :show-inheritance:
   ```

10. **Directory Structure**

    Ensure directory structure looks like this:

    ```
    my_sphinx_project/
    ├── build/
    └── source/
        ├── _static/
        ├── _templates/
        ├── conf.py
        ├── index.rst
        ├── example_module.rst
        └── example_module.py
    ```

11. **Build Documentation**

    Navigate to root directory of Sphinx project where `Makefile` is located and run:

    ```bash
    make html
    ```

    If you make updates, e.g., add new functions like this:

    ```python
    def another_example_function(var1: str, var2: list, var3: float, var4: tuple) -> bool:
        """
        This function performs various operations to demonstrate Sphinx auto documentation.
        
        Parameters:
        var1 (str): The first parameter, a string value to check if it is a palindrome.
        var2 (list): The second parameter, a list of numbers to calculate the sum.
        var3 (float): The third parameter, a floating-point value to check if it is within a specified range.
        var4 (tuple): The fourth parameter, a tuple containing two float values representing the range.
        
        The function processes the input parameters and performs some operations.

        Returns:
        bool: True if all operations are successful, False otherwise.
        """
        # Check if var1 is a palindrome
        if var1 != var1[::-1]:
            return False
        
        # Calculate the sum of var2
        total_sum = sum(var2)
        
        # Check if var3 is within the range specified by var4
        if not (var4[0] <= var3 <= var4[1]):
            return False
        
        # Final check before returning True
        return True
    ```

    Run this in root directory of Sphinx project again:

    ```bash
    make html
    ```

12. **View Documentation**

    Open generated HTML files located in `_build/html` directory.

## Troubleshooting Common Issues

- **"toctree contains reference to nonexisting documents"**:
  - Ensure referenced `.rst` files exist and are correctly named in `toctree` directive.
- **"title underline too short"**:
  - Make sure the underline of section titles in `.rst` files (the `=====`) exceeds the length of the title text.
- **"Autodoc Import Failure"**:
  - If `autodoc` extension fails to import a module due to missing dependencies, install required packages in correct Python environment.
