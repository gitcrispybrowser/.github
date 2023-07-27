## Hi there 👋

<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
open ai : import openai to this file too 


Git/Conversion/pywikibot
Watch
Edit
< Git‎ | Conversion
This page serves as a scratchpad for the pending move of Pywikipedia from SVN -> Git.

Current status
Edit
gerrit/gitblit/github mirroring SVN; updated every hour at :55. [logs]

Conversion code available at https://github.com/pywikibot/svn2git
Project 'pywikibot' on Tools Labs runs run_sync in a crontab


Schedule
Edit
14 june - initial conversion

26 july - complete switch to gerrit

Note. Changes to the conversion code resulting in force pushes to gerrit need to be transferred manually with full_repush, which updates gerrit's references 1000 commits at a time. Large batches make gerrit puke.

Current SVN structure
Edit
/archive/Before_multifamily_changes/pywikipedia/
/archive/init3/pywikipedia/
/archive/init2/pywikipedia/
/archive/init1/pywikipedia/
/archive/trunk/
/archive/messages/
/archive/i18n/
/archive/old python 2.3 scripts/
/branches/rewrite/
/trunk/pywikiparser/
/trunk/pywikipedia/
/trunk/spelling/
/trunk/threadedhttp/
Proposed Git structure
Edit
pywikipedia
/trunk/pywikipedia/ -> master
/branches/rewrite/ -> rewrite
pywikipedia/pywikiparser
/trunk/pywikiparser/ -> master
There are some notes to be made here - we actually have five distinct repositories at the moment: trunk, rewrite, scripts, families and i18n. Scripts, families and i18n are shared (but not consistently) between trunk and rewrite. I would actually like to split it up further somewhat, but git subrepositories are a PITA, so that might not be the best way to go. In any case, I would like to have trunk and rewrite not as two branches, but as two distinct repositories. Valhallasw (talk) 20:29, 2 June 2013 (UTC)[reply]
There is another external named 'simplejson' that when I check out, this is be downloaded. and the repo isn't in wikimedia site and it's in googlecode.com Ladsgroup (talk) 13:08, 10 June 2013 (UTC)[reply]
Proposed Git structure / valhallasw
Edit
pywikibot/core
/branches/rewrite -> master
EXCEPT for i18n
SUBMODULE: scripts/i18n -> pywikibot/i18n
pywikibot/compat
/trunk/pywikipedia -> master
SUBMODULE: i18n -> pywikibot/i18n
pywikibot/i18n
/branches/rewrite/scripts/i18n/ -> master
pywikibot/spelling
/trunk/spelling


Submodules
Edit
As far as I can see, git2svn does not have support for submodules at the moment - so I'm not 100% sure how we can implement the structure above... Ideas:

implementing submodule support (incl. .gitmodules, the mode 160000 directories *and* updates for each upstream change)
implementing partial submodule support (just .gitmodules files)
having the submodules as directories for old commits, then adding a 'remove directory & add submodule' commit after the conversion
no submodules or directories for old commits, then an add submodule commit after the conversion.
Valhallasw (talk) 18:01, 16 June 2013 (UTC)[reply]

Ok, I think we can do a couple of things here. For submodules of things that are also in Gerrit (eg: i18n), Gerrit has support for auto-updating submodules. This will ease the maintenance burden for these repositories and make them behave more like SVN externals (this is what we do for the mediawiki/extensions meta repository, fwiw). For third party projects that are also in Git elsewhere (eg: github), I think we can just use normal submodules. Yes this requires updating manually when an upstream library changes, but this isn't difficult and is generally a good idea (helps track down when upstream broke something). For upstream projects that aren't in Git (eg: SVN or $somethingElse), I think we'll just have to copy the upstream code in manually when it updates. Hopefully this won't be much at all. ^demon[omg plz] 14:47, 17 June 2013 (UTC)[reply]

Submodule fast-export format:

blob
mark :3
data 86
[submodule "svn2git"]
        path = svn2git
        url = https://github.com/pywikibot/svn2git.git

commit refs/heads/master
mark :4
author Merlijn van Deen <valhallasw+lisilwen@gmail.com> 1371755491 +0200
committer Merlijn van Deen <valhallasw+lisilwen@gmail.com> 1371755491 +0200
data 11
+submodule
from :2
M 100644 :3 .gitmodules
M 160000 a3bc2923a139645ada307fd4b53d94dee74da0c1 svn2git
Groups
Edit
pywikipedia - self managing, allow them to add Gerrit users themselves
All users with commit access (Special:Code/pywikipedia/author) should probably get +2? Legoktm (talk) 17:06, 1 June 2013 (UTC)[reply]
Sounds reasonable as an initial list to populate the group with. Like I said, the group will be self-managing, so pywikipedia developers can add new users to it as they'd like. ^demon[omg plz] 12:16, 2 June 2013 (UTC)[reply]
As long as we adhere to a strict '+2 must be from someone else than committer' rule, I'm fine with adding everyone. Otherwise, I'd rather see a smaller group, e.g. xqt, drtrigon and legoktm. The option 'everyone who has committed in 2013' might be even better. Valhallasw (talk) 20:29, 2 June 2013 (UTC)[reply]
Just adding people from 2013 sounds like a better idea. I don't know if we have enough active developers to be able to ensure that everything gets +2'd by someone else... Legoktm (talk) 20:22, 7 June 2013 (UTC)[reply]
2013 sounds fine, that would end up making the following list (new section to ease formatting). ^demon[omg plz] 01:15, 8 June 2013 (UTC)[reply]
Initial members
Edit
alexsh - done
amir - done
my gerrit account is "ladsgroup", i like to change it to amir but It seems it's not possible Ladsgroup (talk) 21:05, 28 June 2013 (UTC)[reply]
Added. Legoktm (talk) 21:08, 28 June 2013 (UTC)[reply]
binbot - done as binaris
btongminh - done
drtrigon - gerrit account "DrTrigon" --DrTrigon (talk) 14:21, 12 July 2013 (UTC)[reply]
done Ladsgroup (talk) 10:21, 13 July 2013 (UTC)[reply]
huji - done
jhsoby - no gerrit account?
I sent an e-mail and asked for making account but, they is not interested.Ladsgroup (talk) 22:52, 18 July 2013 (UTC)[reply]
legoktm - done
malafaya - done
multichill - done
russblau - done
saper - done
siebrand - done
valhallasw - done
xqt - no gerrit account?
account created  @xqt 23:15, 24 July 2013 (UTC)[reply]
added Ladsgroup (talk) 11:05, 25 July 2013 (UTC)[reply]
yurik - done
l10n-bot -- This will be necessary for auto-committing of i18n updates
Current list: [1]

To Do
Edit
MUST be sorted BEFORE migration
Edit
figure out submodule mess
gerrit-wm should report to #pywikipediabot Yes Done with gerrit:70780 and confirmed working. Legoktm (talk) 20:43, 5 July 2013 (UTC)[reply]
gerrit patches (initial patchsets, maybe not each update, and actual commits) should be mailed to pywikipedia-svn
l10n-bot on gerrit
config is in gerrit, at https://phabricator.wikimedia.org/diffusion/GTWN/browse/master/bin/repocommit;849f362c24ac9aaebecb83870ed7eab5ffb80574 / https://phabricator.wikimedia.org/diffusion/GTWN/browse/master/bin/repocreate;849f362c24ac9aaebecb83870ed7eab5ffb80574 / https://phabricator.wikimedia.org/diffusion/GTWN/browse/master/bin/repoexport;849f362c24ac9aaebecb83870ed7eab5ffb80574 / https://phabricator.wikimedia.org/diffusion/GTWN/browse/master/bin/repoupdate;849f362c24ac9aaebecb83870ed7eab5ffb80574
bot needs self-merge rights -- added l10n-bot to pywikibot group, which as 'submit' rights on all pywikibot repositories. Valhallasw (talk) 11:09, 27 June 2013 (UTC)[reply]
automatic submodule update of core and compat
nightly generation (core, core w/ externals, compat w/ externals, spelling)
Yes Done http://tools.wmflabs.org/pywikipedia/
...?
OK after, but should be fixed
Edit
Create .gitreview files in all repos
Done for core and compat in pyrev:11726 and pyrev:11727. Legoktm (talk) 05:41, 8 July 2013 (UTC)[reply]
Github mirroring to pywikibot/pywikibot-core instead of wikimedia/pywikibot-core
Github pull requests
We can use User:Yuvipanda/G2G, would need to ask Yuvi to enable it for pywikibot/* repos. Legoktm (talk) 02:20, 27 June 2013 (UTC)[reply]
I broke gitblit while pushing a new conversion: https://phabricator.wikimedia.org/diffusion/PWBO/
^demon can force a re-import. Best to do this when we are done with force pushing.
...?
Lower priority
Edit
Automated unittests + stuff - would be nice if we had a way to run them on Windows too.
hashar started setting this up, see bugzilla:50302 and gerrit:70845
pep8 and pyflakes linters are set up to run on every patchset.
Also nice would be https://github.com/liamcurry/py3kwarn
bugzilla:50344 is for running python setup.py test on patchsets
Gitblit r### links point to Special:CodeReview/MediaWiki, not pywikipedia.
...?


Distribution
Edit
Currently, changes are distributed to end-users in two ways:

SVN updates
Nightlies
The nightlies can easily switch to a git-based system, but this is less the case for SVN-based users. There are several options we have for this:

Block SVN and force them to switch to git: add a note to wikipedia.py 'we have moved from SVN to git -- please read <this> guide on how to switch!'. This is the method we used when we switched from CVS in 2007: [2]
I'm going with this. I've explained how users can use SVN through github, and I think thats good enough. Legoktm (talk) 04:29, 22 July 2013 (UTC)[reply]
Use a new SVN repository to (essentially) share nightlies. We can switch people by making /trunk/pywikipedia an svn:external to another repository, or by making people use svn switch (not sure if that works)
Something inbetween: an 'upgrade to git' script that (windows) downloads a portable git or (linux) uses the system git to download the from gerrit (could also be used as installer for new users?)
Something inbetween v2: create an update script that downloads & unpacks the latest nightly, and tell people to use that
I wrote a crappy shell script [3] that clones the repo from github and the i18n, and can also update it. It should work fine for any unix user (I only tested on OSX though), we would still need a windows solution though. Legoktm (talk) 05:10, 8 July 2013 (UTC)[reply]
...?
current handler is @sammyfilly
