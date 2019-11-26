---
title: Adopting GitLab workflow
date: 2019-11-30
tags: ["virt", "gnome", "fedora"]
---

In October 2018 there was a face-to-face short meeting with a big part of
libosinfo maintainers, some contributors, and some users.

This short meeting took place during a lunch break in one of KVM Forum 2018
days and, among other things, we discussed whether we should allow, and / or
prefer receiving patches through GitLab Merge Requests.

Here's the [announcement]:
(https://www.redhat.com/archives/libosinfo/2018-December/msg00174.html):
```txt
[Libosinfo] Merge Requests are enabled!

    From: Fabiano Fidêncio <fidencio redhat com>
    To: "libosinfo redhat com" <libosinfo redhat com>
    Subject: [Libosinfo] Merge Requests are enabled!
    Date: Fri, 21 Dec 2018 16:48:14 +0100

People,

Although the preferred way to contribute to libosinfo, osinfo-db and
osinfo-db-tools is still sending patches to this ML, we've decided to
also enable Merge Requests on our gitlab!

Best Regards,
--
Fabiano Fidêncio

```

Now, one year past that decision, let's check what has been done, review some
numbers, and discuss what's my take, as one of the maintainers, of the decision
we made.


### 2019, the experiment begins ...

After the e-mail shown above was sent, I've kept using mailing list as the
preferred way to submit and review patches, keeping an eye on GitLab Merge
Requests, till August 2019 from when I did a full switch to using GitLab
instead of mailing list.


### ... and what changed? ...

Well, to be honest, not much. But in order to extend a little bit more, I have
to describe a little bit my **not optimal** workflow.

Even before describing my workflow, let me just make clear that:

 * I don't have any scripts that would fetch the patches from my e-mail and
   apply them automagically for me;

 * I never ever got used to text-based mail clients (I'm a former Evolution
   developer, I'm an Evolution user for several years);

Knowing those things, this is how my workflow looks like:

 * Development: I've been using GitLab for a few years as the main host of, my
   forks of. the projects I contribute to. When developing a new feature, I'd
   always create a new branch, push the new branch to my GitLab account, and
   finally submit the patches.

 * Review: It may sound weird, maybe it really is, but the way I do review
   patches is by:
    * Getting the patches submitted;
    * Applying atop of master;
    * Doing a `git rebase -i` so I can go through each one of the patchesR;
    * Then, for each one of the patches I would:
      * Add comments;
      * Do fix-up changes;
      * Squash my fixes atop of the original patch;
      * move to the next one;


And now, knowing my workflow, I can tell that pretty much nothing changed.

As part of the development workflow:

 * Submitting patches:

   * [git publish] (https://github.com/stefanha/git-publish) -> click in the
     URL printed when a new branch is pushed to GitLab;

 * Reviewing patches:

   * Saving patch e-mails as mbox, applying them to my tree -> pull the MR

Everything else stays pretty much the same. I still do a `git rebase -i` and
go through the patches, adding comments / fix-ups which, later on I'll have to
organise and paste somewhere (either replying to the e-mail or adding to
GitLab's web UI) and that's the part which consumes the most of my time.

However, although the change was not big **to me** as a developer, some people
had to adapt **their** workflow in order to start reviewing all the patches
I've been submitting to GitLab. But let's approach this later on ... :-)

Anyways, it's important to make it crystal clear that this is my **personal**
experience and that I **do understand** that people who rely more heavily on
text-based mail clients and / or with a bunch of scripts tailored for their
development would have a different, way way different, experience.


### ... do we have more contributions since the switch? ...

As by November 26th, I've checked the amount of submissions we had on
both libosinfo mailing list and libosinfo GitLab page during the current year.

Mind that I'm **not** counting my own submissions and that I'm counting
osinfo-db's addition, which usually may consist in adding data & tests, as a
single submission.

As for the mailing list, we've received **32** patches; as for the GitLab,
we've received **34** patches.

Quite similar number of contributions, let's dig a little bit more.

The **32** patches sent to our mailing list came from **8** different
contributors, and **all of them** had at least one previous patch merged in one
of the libosinfo projects.

The **34** patches sent to our GitLab came from **15** different contributors
and, from those, only **6** of them had at least one previous patch merged in
one of the libosinfo projects, whilst **9** of them were **first time
contributors** (and I hope they'll stay around, I sincerely do ;-)).

Maybe one thing to consider here is whether forking a project on GitLab is
easier than subscribing to a new mailing list when submitting a patch. This is
something people usually do once per project they contribute to, but
subscribing to a mailing list may actually be a barrier.

Some people would argue, though, it's a both ways barrier, mainly considering
one may extensively contribute to projects using one or the other workflow.
IMHO, it's not exactly true. Subscribing to a mailing list, getting the patches
correctly formatted feels more difficult than forking a repo and submitting a
Merge Request.

In my personal case, I can tell the only projects I contribute to which still
didn't adopt GitLab / GitHub workflow are the libvirt ones, although it may
change in the near future, as mentioned by Daniel P. Berrangé on his [KVM Forum
2019 talk] (https://www.youtube.com/watch?v=XTXE-A49-DU).


### ... what are the pros and cons? ...

When talking about the "pros" and "cons" is really hard to get exactly which
are the objective and subjective pros and cons.

 * pros

   * **CI**: The possibility to have a CI running for all libosinfo projects,
     running the tests we have on each MR, without any effort / knowledge of
     the contributor about this;

   * **Tracking non-reviewed patches**: Although this one may be related to
     each one's workflow, it's objective that figuring out which Merge
     Requests need review on GitLab is way easier for a new contributor than
     navigating through a mailing list;

   * **Centralisation**: This is one of the subjective ones, for sure. For
     libosinfo we have adopted GitLab as its issue tracker as well, which makes
     my life as maintainer quite easy as I have "Issues" and "Merge Requests"
     in a single place. It may not be true for different projects, though.


 * cons

   * **Reviewing commit messages**: It seems to be impossible to review commit
     messages, unless you make a comment about that. Making a comment, though,
     is not exactly practical as I cannot go specifically to the line I want
     to comment and make a suggestion.

   * **Creating an account to yet another service**: This is another one of the
     subjective ones. It bothers me **a lot**, having to create an account on
     a different service in order to contribute to a project. This is my case
     with GitLab, GNOME GitLab, and GitHub. However, is that different from
     subscribing to a few different mailing lists? :-)

Those are, for me, the most prominent "pros" and "cons". There are a few other
things that I've seen people complaining, being the most common one related to
changing their workflow. And this is something worth its own section! :-)


### ... is there something out there to make my workflow change easier? ...

Yes and no. That's a horrible answer, ain't it? :-)

Daniel P. Berrangé has created a project called [Bichon]
(https://gitlab.com/bichon-project/bichon), which is a tool providing a
terminal based user interface for reviewing GitLab Merge Requests.

Cool, right? In general, yes. But you have to keep in mind that the project is
still in its embryonic stage. When more mature, I'm pretty sure it'll help
people used to mailing lists workflow to easily adapt to GitLab workflow
without leaving behind the facilities of doing everything via command-line.

I've been using the tool for simple things, I've been contributing to the tool
with simple patches. It's fair to say that it I do prefer adding a comment to
Merge Requests, Approve, and Merge them using [Bichon]
(https://gitlab.com/bichon-project/bichon) than via the web UI. Is the tool
enough to suffice all the people's needs? Of course not. Will it be? Hardly.
But it's may be enough to surpass the blockers of migrating away from mailing
lists workflow.


### ... a few words from different contributors ...

I've decided to ask [Cole Robinson] (https://blog.wikichoon.com/) and [Felipe
Borges] (https://feborg.es/blog/) a word or two about this subject as they are
contributors / reviewers of libosinfo projects.

It should go without saying that their opinions should **not** be taken as
"this workflow is better than the other". However, take their words as valid
points from people who are heavily using one workflow or the other, as [Cole
Robinson] (https://blog.wikichoon.com/) comes from libvirt / virt-tools world,
which rely heavily on mailing list, and [Felipe Borges]
(https://feborg.es/blog/) comes from GNOME world, which is a huge GitLab
consumer.

"The change made things different for me, slightly worse but not in any
disruptive way. The main place I feel the pain is putting review comments into
a web UI rather than inline in email which is more natural for me. For a busier
project than libosinfo I think the pain would ramp up, but it would also force
me to adapt more. I'm still largely maintaining an email based review workflow
and not living in GitLab / GitHub" - [Cole Robinson]
(https://blog.wikichoon.com/)

"The switch to Gitlab has significantly lowered the threshold for people
getting started. The mailing list workflow has its advantages but it is an
entry barrier for new contributors that don't use native mail clients and that
learned the Pull Request workflow promoted by GitLab/GitHub. New contributors
now can easily browse the existing Issues and find something to work on, all in
the same place. Reviewing contributions with inline discussions and being able
to track the status of CI pipelines in the same interface is definitely a must.
I'm sure Libosinfo foresees an increase in the number of contributors without
losing existing ones, considering that another advantage of Gitlab is that it
allows developers to interact with the service from email, similarly to the
email-driven git workflow that we were using before." - [Felipe Borges]
(https://feborg.es/blog/)


### ... is there any conclusion from the author's side?

As the first thing, I've to emphasize two points:

 * **Avoid keeping both workflows**: Although we do that on libosinfo, it's
   something I'd strongly discourage. It's almost impossible to keep the
   information in sync in both places in a reasonable way.

 * **Be aware of changes, be welcome to changes**: As mentioned above,
   migrating from one workflow to another will be disruptive at some level. Is
   it actually a blocker? Although it was not for me, it may be for you. The
   thing to keep in mind here is to be aware of changes and welcome them
   knowing you won't have a 1:1 replacement for your current workflow.

With that said, I'm mostly happy with the change made. The number of old time
contributors has not decreased and, at the same time, the number of first time
contributors has increased.

Another interesting fact is that the number of contributions using the mailing
list has decreased as we only had 4 contributions through this mean since June
2019.

Well, that's all I have to say about the topic. I sincerely hope a reading
through this content somehow helps your project and the contributors of your
project to have a better idea about the migration.
