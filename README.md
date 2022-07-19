<div align="center"><img width="50%" height="50%" src="docs/voila-logo.png" alt="Logo"></div>

`Note: This is a work in progress but is enough to get started with using Voilà
to run jupyter notebooks like an application.`

# What is Voilà?

Voilà is an addition to the Jupyter ecosystem which can be used to run, convert,
and serve a Jupyter notebooks into standalone applications 
and dashboards.

# How does Voilà work?

When Voilà is run on a notebook, the following steps occur:

1. Voilà runs the code in the notebook and collects the outputs

2. The notebook and its outputs are converted to HTML. By default, the
 notebook code cells are hidden.

3. This page is served either as a Tornado application, or via the Jupyter
 server.

4. When users access the page, the widgets on the page have access to the
 underlying Jupyter kernel.


As explained by [Bartosz Telen](https://medium.com/@bartosz.telenczuk?source=post_page-----8a431c2b353e--------------------------------):

    To create interactive and engaging dashboards, you can add graphs, 
    interactive widgets, maps etc. to your notebook. Voilà will turn them into
    interactive applications by stripping any code and displaying the outputs in
    the order they appear in the notebook.

# Local Development Quickstart
```
python -m venv venv
. venv/bin/activate
pip install -r requirements.txt

GIT_URL=example .s2i/bin/run
```

# Deployment

Here is an outline on how use Voilà in OpenShift. We will use this repository 
as our starting template. 

## 1. Fork

Start by forking this repository in to your personal account. We do this so
that we can customized the contents of the repository (i.e. add your notebooks)
## 2. Add your Jupyter Notebooks to the repository

Push your notebooks into the forked repository. We advice that you organized them
in folders. This is because when Voilà serves your notebooks, the contents 
will be organized based on the folder structure of the repository. To help
with the organization of your repository, you could create folders named
`01-Sam`, `02-Sally`, and `03-Tom`. Then instruct your team members to push
their notebooks into their corresponding folder. 

## 3. Update the requirements.txt

The `requirements.txt` is a text file describing the packages needed to 
run your applications. You will need to update it so that it contains 
the dependencies need to run your notebooks. 

## 4. Deploy in OpenShift

Once you reorganized the folder structure, added your notebooks, and made any
necessary updates to `requirements.txt` you are ready to serve your notebooks 
using Voilà in OpenShift. 

Start by deploying Voilà using the s2i technique and the Python base image. 
If you are new to OpenShift, here is a guide on deploying applications using 
the s2i technique:

* [s2i Documentation](https://github.com/openshift/source-to-image)

In here you can see the folder structure of your repository. You can navigate this
application by clicking on the folder names and finding a notebook you want
rendered.

## 5. Customized Deployment

Once the image is built, Voilà is deployed using 

```sh
voila --Voila.ip=0.0.0.0 --port=8080 --template=material
```

This command can be found in `/.s2i/bin/run`. To change the way Voilà is deployed
you will need to change this command. For example to run Voilà in dark mode use:

```sh
voila --Voila.ip=0.0.0.0 --port=8080 --template=material --theme=dark
```

In the S2I build workflow, it is possible to ignore files
and directories by providing a `.s2iignore` file in the root directory of the source
repository. The `.s2iignore` file contains regular expressions that capture the set
of files and directories you want filtered from the image S2I produces. 

In our case, we will like to ignore `media` directory, `.gitignore`, and `README.md`.
To do so, we created a `.s2iignore` file in the root directory with the following contents:

```
media/
.gitignore
README.md
```

Using `.s2iignore` you can prevent junk from going into the build image. 


# References

* [Voilà GitHub](https://github.com/voila-dashboards)
* [Voilà Gallery](https://github.com/voila-gallery)
* [More Voila Examples](https://github.com/pbugnion/voila-gallery)
* [Configure your dashboards with Voilà gridstack template](https://blog.jupyter.org/voila-gridstack-template-8a431c2b353e)