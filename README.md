# Collaborative-reproducible-demo

Using Git(hub), RMarkdown and Google Docs (via the Trackdown package) for a reproducible, collaborative workflow.


## Reproducible *and Collaborative* Workflows

Using Git(hub), RMarkdown and Google Docs (via the `Trackdown` package) for a reproducible, collaborative workflow.

This repo is for you if you are interested in using Rmarkdown, and the Papaja template to draft APA formatted manuscripts and you don't work alone! Unfortunately Rmarkdown documents are not great for writing collaboratively. Rmds can be used to generate Word documents, but then you may be stuck emailing these Word docs between collaborators (with file names that grow as different co-authors edit them). You might choose to convert your Word documents to Google docs, which is great because you can now write collaboratively and make use of the built-in version control and track-changes features instead of emailing, but in either case you will still have a problem: after everyone has finished editing the text, you will need to manually re-integrate the changes into the local copy of your Rmd formatted manuscript. I do not enjoy doing this. The process can be particularly finicky when carrying out small changes throughout a manuscript (e.g. as can happen during rounds of review & resubmit).

One solution to this re-integration problem is to use Git and a service like Github to manage changes. I'm sure this works well in some teams. My own experience is that most of my collaborators don't use Git(hub) and onboarding unwilling collaborators is not worth the pain. I also suspect that for most researchers with low to intermediate GIT skill levels (me!), Git is good for managing code, but not optimised for text.

Another solution is the [`Trackdown` package](https://github.com/ClaudioZandonella/trackdown) package. After a brief detour into why you might want to use Rmarkdown (or Quarto) I then describe my usual workflow with the package, illustrating with a dummy manuscript.

### A quick sidestep: Why Rmarkdown?

Rmarkdown (& quarto) are great for creating dynamic manuscripts. Dynamic manuscripts combine code, the output of that code, and narrative writing all in one file. In a dynamic document, the code is executed within the document in real time and it produces "living" figures, tables, models, etc. that are updated any time the code itself or the underlying data is updated. Because of this, dynamic documents are also reproducible.

The `Papaja` package is an R package that provides document formats to produce complete APA manuscripts from RMarkdown-files (PDF and Word documents) and helper functions that facilitate reporting statistics, tables, and plots.

have collaborators (who may or may not be familiar with Git(hub)). The workflow I describe below is most useful if you want to take advantage of Google docs' version control, track changes and comment features.

In this repository, I document the steps for creating a new project repository, cloning and pushing code changes to Github, and then using the `Trackdown` package to separately create a Google Doc from an RMarkdown file that is used for collaborative narrative work. I am using the `Papaja` package to generate an APA formatted manuscript.

### An example of a typical workflow with a dummy manuscript

This workflow assumes that you use Github, but it's not necessary to do so in order to use Trackdown. If you want to simply work on your local R projects, and then collaborate on the text portions of the manuscripts, you can pick up the Workflow in Step 3.

1.  Create a new project repository in Github

2.  Start a new RStudio project locally by cloning the repository, and create the needed directory structure (e.g. folders for raw data, processed data, manuscript etc.)

3.  Work locally in your project. For this example, I have created a dummy report of a study that uses simulated data in a new rmarkdown file (using the Papaja template). On a "real" project, I would usually start by writing a processing script that outputs cleaned and ready-to-analyse data to the processed data folder, and then (for simple analyses) carry out my analyses within the Papaja generated manuscript.

4.  Once ready to share with collaborators, I use the `trackdown` package to upload my Rmarkdown files to Google drive. (Note that I often do this even if I am the only one writing as I prefer to draft text in Docs. If using Github, make sure to push changes before upload to Drive.)

5.  Install the `trackdown` package from CRAN and use the `upload_file()` function to create a Google doc in your Google Drive from the present Rmarkdown file.

    > trackdown::upload_file(file = "Path-to-file/My-Report.Rmd", hide_code = TRUE)

    Here I have chosen to hide the code so that collaborators don't accidentally overwrite anything they don't understand when we are creating the narrative portion of the text, but I can imagine circumstances (e.g. working with my own PhDs) where I wouldn't want to do this, and specified

6.  The first time you do this, you will have to authenticate your credentials. The message appears in your console, so check, or nothing will happen.

> Is it OK to cache OAuth access credentials in the folder\~/Library/Caches/gargle between R sessions?
>
> 1: Yes
>
> 2: No

7.  Once you've accepted you'll need to navigate to the Google folder you've set as the destination, and you can start working on the Google doc.

    An important point to note is that any changes made to the document in Suggestion Mode, and any comments, need to be accepted before the document is reconciled with the local .Rmd.

8.  Once ready to reconcile the new text with your local .Rmd, run the `download_file()` function from your R session.

> `trackdown::download_file(file = "Path-to-file/My-Report.Rmd")`

After running this in the console, you are duly warned that downloading will overwrite your local file, and have to confirm that you want to do this. Once you agree you are prompted to reload your file and ta-da, the text you (& your collaborators) wrote in Google Docs is now in your local .Rmd document.

For future iterations of this workflow, make sure to use the `update_file()` command instead of `upload_file()`.

#### A few tips for collaborating in Google Docs

1.  Make use of the `path output` parameter in the `upload-file()` and`update_file()` functions to upload the rendered and formatted Word document as well as the google doc version of the markdown formatted manuscript. This will enable collaborators to read through the document with figures, and in-line output in place. (Make sure you have knit immediately before uploading!)

2.  When sharing with collaborators on Google Drive, do not share the whole project folder content with the same permissions. Instead, share the output Word doc as "read only" to ensure that edits do not happen here, and share the Google Doc version of the Rmd with editing permissions.

3.  Set `hide_code = TRUE` in the `upload-file()` and`update_file()` functions if you are working with collaborators who are not code-savvy. This is probably good practice in all cases because anyone reviewing the code should be working from the Github repo, and the Google Drive uploads should be for text edits only.

4.  If there is a lot of data processing in the Rmd, the number of "hidden" code chunks in the Google Doc can start to be visually disorienting. Here a better practice is likely to be creating separate .Rmd or .R files for analyses that are sourced in the main Papaja .Rmd.

5.  Think about how to initiate collaborators to this way of working. I use a brief onboarding document that re-iterates the text `Trackdown` places at the start of the Google Doc to explain how to interact with it, and run through it with collaborators.

6.  When working with a template like Papaja, include 3 separate chunks to use the `Trackdown` commands at the beginning or end of the manuscript. (I find it easier to keep them at the end.) All of these chunks should be set with options `eval = FALSE` and `include = FALSE` so they are not run when you knit the document. For this demonstration document, I've created the following three chunks (named `track-upload`, `track-update` and `track-download`) that I can run manually as needed:

```{r track-upload, eval=FALSE,echo=TRUE}
upload_file(file = "Benefits of petting dogs.Rmd",
  gfile = NULL,
  gpath = "trackdown",
  shared_drive = NULL,
  hide_code = TRUE,
  path_output = "Benefits-of-petting-dogs.docx"
)
```

```{r track-update, eval=FALSE, echo=TRUE}
trackdown::update_file(
  file = "Benefits of petting dogs.Rmd",
  gfile = NULL,
  gpath = "trackdown",
  shared_drive = NULL,
  hide_code = TRUE,
  path_output = "Benefits-of-petting-dogs.docx"
)
```

```{r track-down, eval=FALSE, echo=TRUE}
trackdown::download_file(
  file = "Benefits of petting dogs.Rmd",
  gfile = NULL, 
  gpath = "trackdown", 
  shared_drive = NULL)


```
