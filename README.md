# Swiftype meta tags w/ Saxotech

- [How to](#how-to)
- [References](#references)
- [Examples](#examples)

## How to

First set global variables in the object files for articles, galleries, staff pages, CCE pages, etc...
These variables will most likely be unique for every publication.
In this example, the variables for staff bio pages are set in the `StaffInfo.pbo` object file:

```markup
<%StaffID$V("@StaffID")%>
<%Firstname$V("@StaffFirstName")%>
<%Surname$V("@StaffLastName")%>
<%JobTitle$V("@StaffJobTitle")%>
<%ImgLink$V("@StaffImage")%>
<%Info$V("@StaffInfo")%>
```

You'll then need to cache those variables in a macro.

```markup
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=Edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <pbs:!macro
    name="swiftype-meta"
    cachetime="1440"
    cachequalifiers="@StaffID,@StaffFirstName,@StaffLastName,@StaffJobTitle,@StaffImage,@StaffInfo">
  ...
</head>
```

Finally, in the `swiftype-meta.pbo` file, you can call the variables previously set in the objects to populate your meta tags:

```markup
[%{@StaffID}
  <link rel="canonical" href="<%CCIGlobalBaseURI%>/staff/<%@StaffID$l%>">
  <!-- Swiftype  -->
  <meta class="swiftype" name="title" data-type="string" content="<%@StaffFirstName%> <%@StaffLastName%>, <%@StaffJobTitle%> | ModernHealthcare">
  <meta class="swiftype" name="url" data-type="enum" content="<%CCIGlobalBaseURI%>/staff/<%@StaffID$l%>">
  [%<meta class="swiftype" name="published_at" data-type="date" content="<%Date$d("yyyy-m-d")%>">%]
  <meta class="swiftype" name="info" data-type="string" content="<%@StaffInfo$t%>">
  <meta class="swiftype" name="image" data-type="enum" content="<%CCIGlobalBaseURI%><%@StaffImage%>&imageversion=thumbnail&maxw=240&maxh=240">
%]
```

In that same macro, you can conditionally write your meta tags based on the type of content (after you've defined the variables in their respective object files and added then to the macro's cachequalifiers):

```markup
[%{@StaffID}
  <link rel="canonical" href="<%CCIGlobalBaseURI%>/staff/<%@StaffID$l%>">
  <!-- Swiftype  -->
  <meta class="swiftype" name="title" data-type="string" content="<%@StaffFirstName%> <%@StaffLastName%>, <%@StaffJobTitle%> | ModernHealthcare">
  <meta class="swiftype" name="url" data-type="enum" content="<%CCIGlobalBaseURI%>/staff/<%@StaffID$l%>">
  [%<meta class="swiftype" name="published_at" data-type="date" content="<%Date$d("yyyy-m-d")%>">%]
  <meta class="swiftype" name="info" data-type="string" content="<%@StaffInfo$t%>">
  <meta class="swiftype" name="image" data-type="enum" content="<%CCIGlobalBaseURI%><%@StaffImage%>&imageversion=thumbnail&maxw=240&maxh=240">
%|%[%{@photoName|@photoTitle|@photoCaption|@photoImg}
  <!-- Swiftype  -->
  <meta class="swiftype" name="title" data-type="string" content="[%{@photoName}<%@photoName%> - %]<%@photoTitle$t%> - Modern Healthcare">
  <meta class="swiftype" name="url" data-type="enum" content="<%CCIGlobalBaseURI%>/gallery/<%Date%>/<%Category%>/<%ArtNo%>">
  [%<meta class="swiftype" name="published_at" data-type="date" content="<%Date$d("yyyy-m-d")%>">%]
  <meta class="swiftype" name="info" data-type="string" content="<%@photoCaption$t%>">
  <meta class="swiftype" name="image" data-type="enum" content="[%{@photoImg}<%CCIGlobalBaseURI%><%@photoImg%>&maxw=360&maxh=240%|%https://raw.githubusercontent.com/modernhealthcare/mh-logo/master/mh-logo-twitter.jpg%]">
%]%]
```

## References

- [Basecamp - Saxotech and output of meta tags](https://basecamp.com/2991947/projects/10177881/messages/48081227)
- [Swiftype-specific Meta Tags](https://swiftype.com/documentation/meta_tags2)
- [Saxotech - Global Variables](https://docs.newscyclesolutions.com/display/Onl/Global+variables)

## Examples

> Some example templates from Modern Healthcare's implementation

- [Article.pbo](https://github.com/modernhealthcare/swiftype/blob/master/templates/objects/Article.pbo)
- [ArticleHead.pbo](https://github.com/modernhealthcare/swiftype/blob/master/templates/objects/ArticleHead.pbo)
- [ArticleParagraph.pbo](https://github.com/modernhealthcare/swiftype/blob/master/templates/objects/ArticleParagraph.pbo)
- [ArticlePicture.pbo](https://github.com/modernhealthcare/swiftype/blob/master/templates/objects/ArticlePicture.pbo)
- [StaffInfo.pbo](https://github.com/modernhealthcare/swiftype/blob/master/templates/objects/StaffInfo.pbo)
- [header-default.inc](https://github.com/modernhealthcare/swiftype/blob/master/includes/header-default.inc)
- [metadata.pbo](https://github.com/modernhealthcare/swiftype/blob/master/templates/macros/metadata.pbo)
- [schema.pbo](https://github.com/modernhealthcare/swiftype/blob/master/templates/macros/schema.pbo)
