---
layout: post
title: "Wordpress [Bugs/Hacks/Tweaks/Tricks]? I am not sure"
categories: Hacks
tags: tricks hacks
image: http://www.zee-way.com/templates/default/images/wordpress-logo.png
---

The following is writeup of how I dealt with a specific circumstance. I was up against a wordpress
installation in one of my tests and luckily I already had **editor** level credentials. So I have to somehow obtain a
shell. Remember that editor's do not have the ability to **edit/add themes/plugins**. So, aim was to run php using my
editor privileges.

#### Solid Facts :

+ I can add html in the site using _editor privileges_ (i.e unfiltered_html capability for editors).
+ Nothing more valuable than the above point. (Later found some interesting places in dashboard where payloads work).

#### Methods which won't work :

+ Cannot steal cookies of administrator, since **httponly** flag is set on cookies.
+ No dumb popups asking for password because that might create suspicion, we ain't dealing with skiddies.
+ Solid patched wordpress with up-to-date plugins.

#### Methods which will :

1. Resetting **admin** password. I wrote a sample POC which when run resets admin password to **t3st**.

    ```
    var siteUrl = 'http://wordpress.org' // Wordpress site url

    $(document).ready(function() {
        $.get(siteUrl+"/wp-admin/profile.php?wp_http_referer=%2Fwp-admin%2Fusers.php", function(data) {
            var html = $(data);
            var form = html.find('form');
            form.find('#pass1')[0].value = 't3st'; // New password
            form.find('#pass2')[0].value = 't3st'; // Confirm password
            form.find('#submit')[0].click();
            });
        });
    ```

2. User roles can be changed by admin in the interface, so again with some **js-fu** we can generate these requests to escalate our editor account to admin.
3. Admin can write php directly to themes and plugins, so something like [this](https://nealpoole.com/blog/2011/01/how-does-cross-site-scripting-become-arbitrary-code-execution-an-ode-to-the-oft-maligned-referer-header/) can be done.
4. Email address of the admin can also be changed instead of password, then password reset and boom.

Since I am lazy, I just created POC for the first one. Off all the above, I used the third one to write some file of the theme and I popped a hard **shell**.
So the crisp till now is **Even though wordpress doesn't allow editors to write php directly, we can do it in an indirect manner**

Hold on folks, I know what you are thinking, for the above attacks to work, _a loggedin admin_ has to visit an infected post/page. I will say
not required. **WHY?**

#### Added advantages :

+ A JS-payload entered in the title of a new post/page (by using editor obviously) gets executed right in the homepage of dashboard. **WTF? WHERE?**

    ```
    Recently Published section in the dashboard shows the latest posts/pages, so payloads get executed there
    ```

+ Paylaod also gets executed in **Revisions** of that particular post/page.
+ Obviously the original page/post.

#### And in a flash I remembered two statements :

+ Privilege escalation
+ [Content is never displayed unfiltered in the admin](http://make.wordpress.org/core/handbook/reporting-security-vulnerabilities/#why-are-some-users-allowed-to-post-unfiltered-html)

Reported these to wordpress and they classified these bugs as :

+ Won't Fix
+ N.A

May be they are right, but then why different accounts for *admin* and *editor* when both can do same set of tasks ;)
