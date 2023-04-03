# Problem Statement

* The initial goal is to migrate from the current FTP server onto something that has better support for widespread international access.  FTP access is increasingly denied by browsers and security policies of organizations.
* A secondary goal is to improve workflow.

# Constraints

* We will not change the documentation process (CP, Supplement, DocBook, etc.).  So suggestions like "use LaTeX, Markdown, Sphinx, .... instead of DocBook" will be deferred. (for a long time probably)

* We will limit changes the meeting structure.  The people side (tcons and face-face meetings) should not change.  Support tools like `git` and `gitk` may be needed.

* We will limit total repository size to under 1GB based on performance experience with `git`. (Assuming we use `git`.) The current FTP repositories capture 10+ years of history. 

* We will not make a proprietary package or component necessary unless it is almost universally available.  We use MS-Word, despite the many problems, but don't want to increase our problem level.  (This shouldn't interfere with individual contributors using any proprietary tool.  So all manner of SVG editors can be used, but our process will use SVG.)

# Observations

* Supplements average about 1 MB, and rarely go beyond 100 versions of the supplement during development.  They typically have only a few primary editors.  Other contributions are through markup copies that are integrated by the editors.  The markup copies may be modified word documents, independent documents, text notes, etc.  We need to decide whether those should go into the git archive or not.  Some working groups do archive these, others do not.  Some use email, others do not.

* The CP path is very different than the Supplement path.  Experiments to date have been with supplements.  Separating CP from Supplement may be desirable.

# What should be a repository?

Reasonable considerations were:
1.  One big repository for all of DICOM.  (This fails the size test.  DICOM is much bigger than 1Gbyte)
2.  One repository per working group.  (This works maybe.  WG-06 would have a size problem.  The distribution of the full DICOM standard would have a problem.  We've solved the distribution by using the NEMA web server.)
3.  One repository per supplement, and some other path for dealing with meeting folders and CPs.

Given what I've seen so far, I conclude:

* There should be a different repository per supplement.  

    * If a working group generates 25 versions (internal, wg-06 shared, PC, LB, FT) and over ten year period they generate 40 supplements we hit the 1 GB limit.  So really busy working groups might hit the lemit, although most will not come close.

    * It's hard to manage many authorized writers over a long period of time on the commercial servers unless we pay for enterprise accounts, and then get all the working group participants to become members of the enterprise account.  This looks expensive and burdensome for NEMA staff.

    * It's easy to manage pull requests from many free/personal/other accounts as an authorized editor.  But, the clone and pull become burdensome if it clones and pulls the entire 10+ year history a a working group.  It's reasonable and not too burdensome to clone and pull the entire history of one supplement.  Some supplements have been monsters, but those are the ones where you are most often going to need to see the entire history.

* The approach for CPs and meeting folders needs more thought.