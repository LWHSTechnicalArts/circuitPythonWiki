# Tech Arts CircuitPython Wiki
This site describes each library in the CircuitPython bundle in an inviting way, to enable students to design and invent more fluently.

## Contribution
1. Clone this repository
2. Create a new .md file in the docs folder
3. Push to main

Your push to main triggers a GitHub workflow that consists of a single deploy task. This task:
1. starts an Ubuntu virtual machine
2. installs Python
3. installs mkdocs and a mkdocs theme
4. builds the site with mkdocs
5. deploys the site to the gh-pages branch

This will take a moment. The last commit banner above the files will show you an orange dot while the deploy finishes, and then a green check mark. The "Actions" tab (betwen "Agents" and "Projects") will also show you a deploy history.

Visit https://lwhstechnicalarts.github.io/circuitPythonWiki/ to access the deployed site.

## To Iterate Faster
You can serve a version of this wiki locally for quicker design iteration.

    pip install mkdocs
    pip install mkdocs-material
    mkdocs serve
    visit localhost:8000

# References

1. [GitHub Workflows](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows)

2. [MkDocs](https://www.mkdocs.org/)