# Contribution Guidelines

Follow this guide to start contributing to the ERC Research Notes site.

## Setting up

* Fork the repository to your github account by pressing the 'Fork' button on the top right corner of the screen when you open the repo

* Clone your fork to your computer using :

    ```bash
    git clone https://github.com/your-github-username/research-notes.git
    ```

* Installing dependencies:

    ```bash
    python3 -m pip install -r requirements.txt
    ```

## Ways of Contributing

### 1. Openning new issues

You can open issues from the issues page of the repo. Issues can be opened if you:

* Have ideas for new things that can be added to the handbook

* Find any errors / mistakes in handbook

* General suggestion on how the handbook can be improved

### 2. Solving Issues

You can start working on an unsolved issue by requesting to take it up in the [Issues Section](https://github.com/ERC-BPGC/research-notes/issues) of the repo on github. The issue will then be assigned to it and you are good to go.

After the issue is assigned to you, you can start working on it by setting up a local
repo by following the steps giving in the configuration section.

Remember to update your local repo before starting the work everytime by using :

```bash
git pull upstream main
```

At this stage you will be ready to make your changes.

## Modifying the handbook

We are using the [mkdocs-material](https://squidfunk.github.io/mkdocs-material/getting-started/#configuration) theme of [mkdocs](https://www.mkdocs.org/) to build the handbook. The documentations of both tools have many details regarding further configuration etc.

All the documentation is written in the form of [markdown](https://www.markdownguide.org/getting-started/) files which are contained within the `/docs/` directory and are sorted according to the field of robotics that the content of the file pertains to. For ways to do cool things with markdown like callouts, tables, emojis etc. check out [this](https://squidfunk.github.io/mkdocs-material/reference/admonitions/) page.

We are using [Python Markdown](https://facelessuser.github.io/pymdown-extensions/extensions/)'s [Arithmatex](https://facelessuser.github.io/pymdown-extensions/extensions/arithmatex/) extention for rendering [mathjax](https://www.mathjax.org/) (this includes [latex](https://www.latex-project.org/)) for which you can find more details [here](https://squidfunk.github.io/mkdocs-material/reference/mathjax/).

Adding a new page to the handbook involves two steps -

1. Create the `.md` file for the page in the relevant section.
2. Modify the `nav` section of `mkdocs.yml` to contain a reference to the name of the new file. As an example, if you want to add content to the _Controls_ sub-section then modications to the yml file will be as follows :

```yaml
nav :
    - Controls :
      - BEECLUST Swarm Algorithm:path/to/beeclust.md
      - Your contribution: path/to/file_name.md
```

To check how your page looks when built, you can run `mkdocs serve` from the root directory of the project. The cool thing is the page will autoupdate as you make changes to the `.md` source file.

Once you feel your addition is ready, you should stage and commit your changes and push to your fork.

```bash
git add .
git commit -m "Commit Message"
git push origin main
```

After this all you have to do is [open a pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request). While opening a pull request please make sure that the title of your PR is of the form ```[DD/MM/YY] Nice framed message describing the changes```. Here, we have set up an automated test to see if the docs in the PR build, so make sure your contribution passes that.

Since, we are using a GitHub Action to deploy the docs, whenever there are any updates to the `main` branch, the website will be automatically updated.
