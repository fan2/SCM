
# bridge & migration

## bridge to SVN

[Using Git with SVN repository – simple case howto and benefits](http://sergtk.com/blog-it/2014/git-benefits-with-svn-repository)

### [git help svn](https://git-scm.com/docs/git-svn)

```
GIT-SVN(1)                                  Git Manual                                 GIT-SVN(1)



NAME
       git-svn - Bidirectional operation between a Subversion repository and Git

SYNOPSIS
       git svn <command> [<options>] [<arguments>]


DESCRIPTION
       git svn is a simple conduit for changesets between Subversion and Git. It provides a
       bidirectional flow of changes between a Subversion and a Git repository.

       git svn can track a standard Subversion repository, following the common
       "trunk/branches/tags" layout, with the --stdlayout option. It can also follow branches and
       tags in any layout with the -T/-t/-b options (see options to init below, and also the
       clone command).

       Once tracking a Subversion repository (with any of the above methods), the Git repository
       can be updated from Subversion by the fetch command and Subversion updated from Git by the
       dcommit command.
```

### SmartGit

[SmartGit as SVN Bridge](https://www.syntevo.com/doc/display/SG/SmartGit+as+SVN+Bridge)  

an introduction to SmartGit from an SVN users perspective

[A bridge to Subversion](https://www.syntevo.com/smartgit/integrations/svn/)  

SmartGit combines the best of both worlds: the power of a **local Git repository** with the access control of a central **Subversion server**.

## migration

### SmartGit(SubGit)

[SVN to Git migration](http://smartgit.3668570.n2.nabble.com/SVN-to-Git-migration-td7575108.html) - discussion  

[SubGit - Smooth team migration from SVN to Git](https://www.smartsvn.com/subgit/)  

`SubGit` is a tool for a smooth, stress-free SVN to Git migration.

> [svn_mirror_presentation](https://subgit.com/files/svn_mirror_presentation.pdf) - PDF  

### command line

[Converting a Subversion repository to Git](https://john.albin.net/git/convert-subversion-to-git)  

GitStack - [Migrate from svn to git](https://gitstack.com/migrate-from-svn-to-git/)  
