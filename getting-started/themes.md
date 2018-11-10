# Themes

--------------------------------------------------------

- [Overview](#overview)
- [Requirements](#requirements)
- [Architecture](#architecture)
	- [OOP](#oop)
	- [Understanding MVC](#understanding-mvc)
	- [Push-Pull MVC](#push-pull-mvc)
- [Folder structure](#folder-structure)
- [Load behavior](#load-behavior)

--------------------------------------------------------

Kanso Themes allow you to build a completely customized website that is self contained within the Kanso environment.

Theming allows you to customize the functionality of your website quickly and easily, with a minimal amount of code.

Kanso Themes follow a similar design pattern and architecture as WordPress, so if you've worked with WordPress in the past, you'll find migrating very straight-forward.

--------------------------------------------------------

### Overview

Kanso Themes live in subdirectories of the default themes directory `app/public/themes`. The Theme's subdirectory holds all of the Theme's style-sheet files, template files, optional functions file (`functions.php`), JavaScript files, and images.

For example, a Theme named "test" would reside in the directory `app/public/themes/test/`. A valid Kanso theme folder should contain no spaces or special characters - only alphabetic characters and hyphens. For example a theme found in `app/public/themes/my custom theme` is invalid.

Currently, Kanso does not ship with a default theme, you will need to follow this documentation carefully to create your own.

--------------------------------------------------------

### Requirements

As flexible as Kanso is, simplicity is still one of it's major objectives. For a Theme to be considered a valid Kanso Theme, it must follow certain guidelines - Mainly these are just the requirements of PHP files.

For a theme to be valid, the following files MUST be present in the theme's directory:

- index.php
- single.php
- page.php
- taxonomy.php
- search.php


Additionally, the following optional files have direct support by Kanso:

- functions.php
- header.php
- footer.php
- sidebar.php
- searchform.php
- commentform.php
- comments.php
- attachment.php
- 404.php
- sitemap.php

Kanso also has support for wildcard template loading using the following files:

- homepage.php
- home-`$slug`.php
- blog.php
- page-`$slug`.php
- single-`$slug`.php
- single-`$post-type`.php
- tag-`$slug`.php
- taxonomy-tag.php
- category-`$slug`.php
- taxonomy-category.php
- author-`$slug`.php
- taxonomy-author.php

Beyond this, it is up to you to structure your theme architecture as you see fit. You can create as many files as you like and use Kanso's include_template method to require templates as necessary.

--------------------------------------------------------

### Architecture

Before we go into more specifics about theme folder structures it's important that you understand some major concepts as this will help you have a better understanding of where Kanso sits in the realm of architecture and design patterns.

--------------------------------------------------------

### OOP

If you've been reading this documentation carefully, you will notice it's been mentioned at various points that Kanso uses a OOP (Object Oriented Programming) Push-Pull theming system. Simply put OOP is a design pattern or architecture that focuses on keeping tasks as modular as possible. In OOP any module (class) should be able to function independently from the rest of the application. While this is certainly not the case for all of Kanso's classes, Kanso follows this concept as closely as possible.

With Kanso each file always contains a single class. And each class follows a namespace that is an exact representation of it's directory location and file name.

--------------------------------------------------------

### Understanding MVC

Whether you're a back-end ninja or noob, understand core concepts is important. To help better understand how Kanso works, it's important to understand the MVC (model–view–controller) design pattern. Let's go through it (briefly).

As far as we're concerned MVC is a design pattern the handles HTTP requests (eg. you when you go to `www.example.com` and your browser asks a server for a page). 
Typically a single controller handles the request (e.g `index.php`), however for more complicated applications there might be a number of different controllers depending on the request.

The controller's task is to dispatch the request by loading and communicating with the appropriate model. The model's job is to validate the request and build variables and content.

The content then gets sent to the view via the controller. The view is what gets output to the client (ie. the browser). The view's task is to process any data (variables) sent to it and display them.

It may be easier to understand the process like this (assuming the request is valid):

- A request is made for www.example.com/page.
- A router may be used to match the request and load the appropriate controller.
- The controller loads the model and asks it if the request is valid.
- The model runs some validation and responds to the controller.
- The controller asks the model to build the required data to be sent to the view.
- The model does a bunch of work and responds to the controller with some data.
- The controller loads the appropriate view with the data.
- The view is sent to the client.

--------------------------------------------------------

### Push Pull MVC

Now that we understand how MVC architecture works. Let's look at push and pull and how they effect MVC. 
There's a great explanation on push-pull MVC [here](http://www.guyrutenberg.com/2008/04/26/pull-vs-push-mvc-architecture/) by Guy Rutenberg:

> Push and pull describe the relation between the View and the Controller. According to the push model, user actions should be interpreted by the Controller which will generate the data and push it on to the View, hence the “push”. On the other hand, the pull model assumes that the user requires some kind of output (like a list items from the database). The View will access the Controller in order to get the data it needs in order to display the user the kind of output he requested. This is much like the View is “pulling” data from the Controller.

Now that we understand MVC architecture, we can better understand themeing with Kanso. When you're working with a theme in Kanso, you are working directly inside the view. It's your job as a developer to pull data from Kanso itself via the CMS and framework.

--------------------------------------------------------

### Folder structure
As discussed above, building a theme folder structure is your responsibility as a developer. A typical Kanso Theme folder structure might look like this:

`exampleTheme
├── 404.php
├── assets
│   ├── css
│   │   └── style.css
│   ├── img
│   │   └── favicon.png
│   └── js
│       └── scripts.js
├── commentform.php
├── comments.php
├── footer.php
├── functions.php
├── header.php
├── index.php
├── taxonomy.php
├── page.php
├── search.php
├── searchform.php
├── sidebar.php
└── single.php`

You can create as many files as you like and use Kanso's `include_template` method to load them as necessary.

--------------------------------------------------------

### Load behavior

When a request comes in, Kanso will try to to load specific template files first (if they exist). Otherwise the application will continue through to more general templates until one is found. The table below illustrates this load behavior more specifically. If no template is found Kanso's middleware will be invoked until finally a 404 response is sent (if nothing else changed the response object in the meantime).


| Request Type     | Example URI             | Template order                                                                |
|:-----------------|:------------------------|:------------------------------------------------------------------------------|
| Homepage         | /                       | `homepage.php -> index.php`                                                   |
| Blog Location    | /slug/                  | `home-$slug.php -> blog.php -> index.php`                                     |
| Single Page      | /slug/                  | `page-$slug.php -> page.php`                                                  |
| Single Post      | /05/2017/slug/          | `single-$slug.php -> single.php`                                              |
| Custom Post Type | /custom/slug/           | `single-$slug.php -> single-$type.php -> single.php`                          |
| Tag              | /tag/slug/              | `tag-$slug.php -> taxonomy-tag.php -> tag.php -> taxonomy.php`                |
| Category         | /category/slug/         | `category-$slug.php -> taxonomy-category.php -> category.php -> taxonomy.php` |
| Author           | /author/slug/           | `author-$slug.php -> taxonomy-author.php -> author.php -> taxonomy.php`       |
| Search Results   | /search-results/?q=test | `search.php -> index.php`                                                     |
| Attachment       | /attachment/slug.ext    | `attachment-$slug.php -> attachment.php -> default-attachment`                |
| Sitemap          | /sitemap.xml            | `sitemap.php -> default-sitemap`                                              |

