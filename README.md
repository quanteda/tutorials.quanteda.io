# quanteda tutorials website

[quanteda tutorials website](https://tutorials.quanteda.io) is created by Hugo based on the [Learn theme](https://github.com/matcornic/hugo-theme-learn). Since Hugo accepts only Markdown and HTML, we use [**blogdown**](https://github.com/rstudio/blogdown) to generate those files from Rmarkdown. 

## How to add new pages?
You can add new pages to the `content` folder, but note that the file extension must be `.Rmarkdown` not `.rmd`, because 
**blogdown** converts `.rmd` to `.html` and `.Rmarkdown` to `.markdown`. After adding pages, you probably have to rebuild the website by clicking the 'Build Website' button in the build panel in R Studio.

## How to edit pages?
If you have **blogdown** installed, you can execute `blogdown::serve_site()` in the console to check how your pages will look like. It will start a web server on your local machine so that you can preview the changes in your browser. HTML files are saved in the 'public' folder.

## How to update files on the server?
After editing the website locally, commit all the changes and push them to the master branch. [Netlify](https://www.netlify.com) will then detect the changes and update the file on the server for you within a minute or so.
