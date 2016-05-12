# Rest Drupal Simple Article Module

## Purpose

This module contains:

- Configuration for the Simple Article content type
- REST export of Simple Articles
- Creates dummy users and content on install 


### Simple Article Content Type

The Simple Article content type is quite simple! It has the following additional
fields:

- Body 
- Author (entity reference to a single User entity -- autocomplete is sorted by 
username, ascending)

The Author field exists since typically the creator of the node is not always
the "author" of the content. Adding this field covers this typical use case.


### REST export of Simple Articles

The view configuration is at: `/admin/structure/views/view/rest_drupal_simple_articles`

The path for the JSON list of Simple Articles is: 

- `/api/v1/articles`
- `/api/v1/articles/{id}`

If the optional `id` (a context filter) is given, then the returned JSON will be
of just the matching Simple Article node, if it exists. Non-matches return: `[]`.

Published Simple Articles are exposed in this list and sorted by created dated, 
descending.
 
The restriction to this content is only based on the "Viewing published content"
permission -- the same permission used for allowing access to published nodes 
on site. 


### Creates dummy users and content on install

Creates dummy users. Also creates a few Simple Article nodes based on the values
in the `rest_drupal_simple_article/files/dummy_articles.csv`. See Caveats section
below for an explanation of an alternative or more deploy-friendly way of 
accomplishing this. 


## Caveats

Due to the simplicity of this illustration, there are the following caveats:

1. Devel Generate can certainly be used to generate Users and Content. The only
issue that's currently the case with Devel Generate for Content is that the
summary of a body field tends to be quite long.
2. Dummy users and roles should be managed in a separate module rather than being
handled in the same module as the content type module. Typically a role (like 
Editor) should be defined and any dummy content creator users should be assigned
this role rather than being granted the Administrator role. But for the sake of
simplicity this has been skipped.
3. Creation of dummy users and dummy content should be an optional part of a
deploy process, instead of being packaged in an install hook. The csv file would
be placed outside of the module. This means that these should be defined as drush 
commands (again, devel generate could certainly be used). If demo content must 
be created on install (or optionally via install), it should be separated out 
into a separate demo content module.
