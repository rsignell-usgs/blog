#Creating your own IPython Notebook Blog environment on Wakari

* Check to make sure pandoc is available.  You can't convert the notebooks to blog entries without it. Open a terminal in Wakari, and type `which pandoc`.  It should return something, like `/opt/anaconda/bin/pandoc`.   If not found, contact  wakari_support@continuum.io and ask them to install pandoc.

* Go to https://github.com/rsignell-usgs/blog and click the `fork` button to fork the blog repository.
* [Create a SSH keypair to use for pushing from Wakari to Github pages](https://help.github.com/articles/generating-ssh-keys) except you won't be able to do the testing in step 4 until you add the lines below to your .bashrc and start the ssh agent.  I didn't use a passphrase on SSH (I just hit enter when it asks for a passphrase).


* In Wakari, go to "terminals", then select "shell" and add these lines to your `$HOME\.bashrc` to start SSH agent when you login to Wakari:
```
ssh-agent | tee ~/.ssh/agent.sh
source ~/.ssh/agent.sh
ssh-add ~/.ssh/id_rsa
```
* Either create a new terminal, or execute this by typing `source ~/.bashrc`
* In a Wakari terminal, make a new directory "blog" and clone your blog repository from your fork using SSH, for example:
```
mkdir $HOME/blog
cd $HOME/blog
git clone git@github.com:dpsnowden/blog.git
```

* You should now have a `$HOME/blog/blog` directory.
* Edit the `pelicanconf.py` and `publishconf.py` files and customize for your settings. 
* Edit the `./octopress-theme/templates/_includes/{about.html, twitter.html}` files and customize for your settings.

* Create a custom Wakari `blog` environment with the necessary conda packages:
```
cd $HOME/blog/blog
conda create --name blog --file blog.spec
```
Note: this conda "spec" file I created by just doing: 
```
source activate blog
conda list -e | tee blog.spec
```
so it may contain some extra packages.

* Activate the `blog` environment:  
```
source activate blog
```

* Install the non-conda packages via pip:
```
pip install -r requirements.txt
```

* New notebooks go in `$HOME\blog\blog\content\downloads\notebooks`
* Create a new markdown file in `$HOME\blog\blog\content` that points to the new notebook.  These entries look like this:

```
cd $HOME\blog\blog\content
more 2014-01-15_NGDC-CSW.md

Title: CSW Access to NGDC OPeNDAP Endpoints
date:  2014-01-15 10:08
comments: true
slug: csw_ngdc_dap

{% notebook NGDC-CSW-DAP.ipynb cells[2:] %}
```
* In the directory `$HOME\blog\blog`, type `make github` to convert notebooks to html and push to gh-pages
