---
layout: post
title: Creating my GitHub repo list site using GitHub API
description: Creating dedicated site for listing GitHub repositories and gists for better clarity especially where there are a lot of repos/gists.
keywords: gird layout, github repo list site, github gist list site, listing site, github api v3, jquery ajax, muuri plugin
tags: [JavaScript, AJAX, GitHub API]
comments: true
---

Sometimes, I found that the best way to display all of my GitHub repositories for better clarity is to create its own dedicated site for that. Well, especially when I have a lot of repos, it will be quite helpful to look all those repos in one glance. Like [Twitter](https://twitter.github.io/), [IBM](https://ibm.github.io/) or [Uber](https://uber.github.io/) did. All of these are possible by using [GitHub API](https://developer.github.com/v3/).

Previously, my repo list dedicated site was using the same layout as I did for [my gists](http://heiswayi.github.io/my-gists/). Below is the screenshot for my gist list dedicated site, and for now I don't plan to change the site interface. Oh if you never heard or don't know what is gist, actually [gist](https://help.github.com/articles/about-gists/) is a great service provided by GitHub to offer a great way to share your work. It can be a single files, parts of files or full application. Best place to save or share your code snippet too!

![my-gists](http://i.imgur.com/5OpkLPM.png)

**Site URL:** [heiswayi.github.io/my-gists](http://heiswayi.github.io/my-gists/)

### My new GitHub repo listing site

Now it's 2017 and I thought I should change my repo list site layout to a new interface, and today I did it!

![my-repos](http://i.imgur.com/KYNh11h.png)

**Site URL:** [heiswayi.github.io/my-repos](http://heiswayi.github.io/my-repos/)

This new site layout provides more clarity and also supports pagination if my repos count reaches 100+ repos as [GitHub Search API only provides items in sets of 100](https://developer.github.com/guides/traversing-with-pagination/). It's build from scratch and passion using [jQuery](https://jquery.com/) (working alot with AJAX request), [Bulma - A modern CSS framework based on Flexbox](http://bulma.io/), [Font Awesome](http://fontawesome.io/), [Muuri plugin](https://haltu.github.io/muuri/) and [GitHub API v3](https://developer.github.com/v3/).

### Example of code snippets

Mostly I query the GitHub API using jQuery `$.ajax` and process the return response data received in JSON format.

```js
var github_username = 'heiswayi';
var per_page = 100;

$.ajax({
    type: 'GET',
    url: 'https://api.github.com/users/' + github_username,
    dataType: 'json',
    success: function(data) {
        // do something JSON response data
        // for example, get total number of my repos:
        var total_repos = data.public_repos;

        // now, get all repos (max 100 repos):
        get_repos('https://api.github.com/users/' + github_username + '/repos?&per_page=' + per_page);
    }
});

function get_repos(api) {
    $.ajax({
        type: 'GET',
        url: api,
        dataType: 'jsonp',
        success: function(data) {
            // console.log(data);
            init(data); // initialize Muuri plugin
        }
    });
}
```

Initialization of [Muuri plugin](https://haltu.github.io/muuri/) and item iterations:

```js
function init(data) {
    if (!data || !data.data || !data.data.length) return;
    if (!grid) {
        grid = new Muuri({
            container: $grid.get(0),
            items: generateElements(data),
            dragEnabled: false,
            dragReleaseEasing: 'ease-in',
            dragContainer: document.body,
            layout: ["firstFit", {
                fillGaps: true
            }]
        });
    }
}

// here is the example of how I generate each repo info
function generateElements(data) {

    var repositories = data.data;

    // sort last modified repo to the most recents
    repositories.sort(function(a, b) {
        var aFork = a.fork,
            bFork = b.fork;
        if (aFork && !bFork) return 1;
        if (!aFork && bFork) return -1;
        return new Date(b.pushed_at) - new Date(a.pushed_at);
    });

    var ret = [];

    for (var i = 0; i < repositories.length; i++) {

        var r = repositories[i];

        var id = ++uuid;
        var desc = r.description == null ? '' : r.description;
        var lang = r.language == null ? 'unknown' : r.language;
        var size = r.forks >= 100 ? 'is-6' : 'is-3';
        var isFork = r.fork == true ? '<i class="fa fa-code-fork" title="Forked repository"></i> ' : '<i class="fa fa-github-alt" aria-hidden="true"></i> ';
        var isRepo = r.fork == false ? 'repo' : 'forked';

        var item = $('<a href="' + r.html_url + '" class="item column ' + size + '" title="Click to go to repository on GitHub...">' +
            '<div class="item-content notification ' + isRepo + '">' +
            '<span class="name">' + isFork + r.name + '</span>' +
            '<span class="desc">' + desc + '</span>' +
            '<span class="star" title="Number of watchers"><i class="fa fa-star" aria-hidden="true"></i> ' + r.watchers + '</span><span class="spacing-20"></span><span class="fork" title="Number of forks"><i class="fa fa-code-fork" aria-hidden="true"></i> ' + r.forks + '</span><span class="spacing-20"></span><span class="language" title="Major programming language"><i class="fa fa-code" aria-hidden="true"></i> ' + lang + '</span>' +
            '</div>' +
            '</a>').get(0);

        ret.push(item);
    }
    return ret;
}
```

### It's open source!

Well, if you explore more on the source code, or you want to use this site source code directly for your own GitHub repo list, feel free to do so. It's open source and licensed under [MIT License](http://heiswayi.github.io/mit-license). **You may fork or download the source code on [GitHub](https://github.com/heiswayi/my-repos).**

**Similar to [my-gists site](http://heiswayi.github.io/my-gists), the [source code](https://github.com/heiswayi/my-gists) is available on GitHub too!**
