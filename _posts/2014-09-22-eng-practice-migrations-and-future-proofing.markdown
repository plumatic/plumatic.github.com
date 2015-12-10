---
layout: post
title: 5 Ways to Change Your APIs and Data Formats without Downtime
date: 2014-09-22 10:53:24.000000000 -07:00
---
##Eng Practice: Migrations and Future Proofing

You've implemented a new feature, which involved refactoring some existing data formats or server handlers. Everything looks good, the tests all pass, and (if applicable) the latest clients (frontend and/or backend) all seem to work with the latest servers. Ship it?!

Not so fast.

If your changes involve formatting changes to data at rest, and/or interface changes to servers with clients running in other processes, you also have to worry about (and test) how your changes will interact with old data or code.

In our latest eng practice, we'll talk about the issues (inertia), how to deal them when they come up (migrations), and have a discussion of best practices (forward compatibility) to reduce the need for or difficulty of future migrations.

[Find out how Prismatic refactors without downtime...](https://github.com/Prismatic/eng-practices/blob/master/swe/Migrations_and_future_proofing.md)

<hr style="border-top: 1px solid #ccc">
Want to stay up-to-date with everything going on with the Prismatic Engineering team? Sign up below to receive periodic updates (no spam, we promise!)

<!-- Begin MailChimp Signup Form -->
<link href="//cdn-images.mailchimp.com/embedcode/slim-081711.css" rel="stylesheet" type="text/css">
<style type="text/css">
	#mc_embed_signup{background:#fff; clear:left; font:14px Helvetica,Arial,sans-serif; }
	/* Add your own MailChimp form style overrides in your site stylesheet or in this style block.
	   We recommend moving this block and the preceding CSS link to the HEAD of your HTML file. */
</style>
<div id="mc_embed_signup">
<form action="http://getprismatic.us8.list-manage2.com/subscribe/post?u=48f886b5d37625b7ce4f1c568&amp;id=a81a653780" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" style="padding-left:0;" novalidate>
	
	<input type="email" value="" name="EMAIL" class="email" id="mce-EMAIL" placeholder="email address" required>
    <!-- real people should not fill this in and expect good things - do not remove this or risk form bot signups-->
    <div style="position: absolute; left: -5000px;"><input type="text" name="b_48f886b5d37625b7ce4f1c568_a81a653780" tabindex="-1" value=""></div>
    <div class="clear"><input type="submit" value="Subscribe" name="subscribe" id="mc-embedded-subscribe" class="button"></div>
</form>
</div>

<!--End mc_embed_signup-->
