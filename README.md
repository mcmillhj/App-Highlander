[![Build Status](https://travis-ci.org/mcmillhj/App-Highlander.svg?branch=master)](https://travis-ci.org/mcmillhj/App-Highlander)
[![Coverage Status](https://coveralls.io/repos/mcmillhj/App-Highlander/badge.svg?branch=master)](https://coveralls.io/r/mcmillhj/App-Highlander?branch=master)
[![Kwalitee status](http://cpants.cpanauthors.org/dist/App-Highlander.png)](http://cpants.charsbar.org/dist/overview/App-Highlander)

# NAME

App::Highlander - Module that provides simple named locks

# VERSION

version 0.003

# SYNOPSIS

    use App::Highlander; 

    App::Highlander::get_lock();

    # ...

    App::Highlander::release_lock(); 

or 

    use App::Highlander; 

    my $has_lock = App::Highlander::get_lock('lockstring');
    while ( ! $has_lock ) {
       sleep 10;
       $has_lock = App::Highlander::get_lock('lockstring');
    }
    # ... code ...

    App::Highlander::release_lock('lockstring');

or

    use App::Highlander LOCKDIR => "$ENV{HOME}/.locks";
    
    ...

# DESCRIPTION

Simple module that provides a named locking mechanism based on flock. Application code requests a lock, then executes, then releases the lock. Lockfiles are stored in a temporary directory created by File::Temp::tempdir( CLEANUP => 1 ) by default. You can optionally use a different LOCKDIR directory when using App::Highlander by specifying the LOCKDIR option. Keep in mind that any directory you specify needs to exist and the user running the Highlander'd script needs to have write and read permisissions on files in that directory.

App::Highlander does \*not\* currently (and may never) handle errors, this means that if your application dies under Highlander then it will not have released the lock. Application code will need to capture the error with eval or a sugary module like Try::Tiny then explicitly release the lock.

# NAME

App::Highlander

# METHODS 

- `get_lock`

    Attempts to get a lock on the supplied lock string. If no lock string is supplied then $PROGRAM\_NAME will be used. Locks are written to a temporary directory or LOCKDIR if one was specified
    Returns the name of the lock file that was created.

- `release_lock`

    Attempts to release a lock on the suppplied lock string. If no lock string is supplied then $PROGRAM\_NAME will be used. Releasing the lock will close the open filehandle is that was used for flock and delete the lockfile
    Returns the name of the lock file that was destroyed.

# ISSUES

Issues (bugs or feature requests) should be created on the github repo: https://github.com/mcmillhj/App-Highlander/issues

# AUTHOR

Hunter McMillen &lt;mcmillhj@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2015 by Hunter McMillen.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
