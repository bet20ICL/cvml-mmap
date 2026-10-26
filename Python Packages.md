
What happens when interpreter sees `import numpy`?

1. Searches folders on `sys.path`
2. This includes `site-packages`, where most packages are installed
3. Installing a package means putting some code into `site-packages`

[[Wheels]]
- a zip of Python files and any compiled code e.g. `.so` files

[[PyPI]]
- `pip` is just a tool for downloading wheels from a shared repository and installing them into `site-packages`

[[Conda]]
- 