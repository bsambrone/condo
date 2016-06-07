---
layout: docs
title: gem
group: shades
---

@{/*

gem
    Executes the gem command line utility.

gem_args=''
    Required. The arguments to pass to the gem command line tool.

gem_options='' (Environment Variable: GEM_OPTIONS)
    Additional options to use when executing the gem command.

gem_path='$(base_path)'
    The path in which to execute gem.

gem_wait='true'
    A value indicating whether or not to wait for exit.

gem_quiet='$(Build.Log.Quiet)'
    A value indicating whether or not to avoid printing output.

base_path='$(CurrentDirectory)'
    The base path in which to execute gem.

working_path='$(base_path)'
    The working path in which to execute gem.

*/}

use namespace = 'System'
use namespace = 'System.IO'

use import = 'Condo.Build'

default base_path         = '${ Directory.GetCurrentDirectory() }'
default working_path      = '${ base_path }'

default gem_args          = ''
default gem_options       = '${ Build.Get("GEM_OPTIONS") }'

default gem_path          = '${ working_path }'
default gem_wait          = '${ true }'
default gem_quiet         = '${ Build.Log.Quiet }'

default ruby_install_path = '${ Build.Get("RUBY_INSTALL_PATH") }'

ruby-locate once='ruby-locate'

@{
    Build.Log.Header("gem");

    if (string.IsNullOrEmpty(gem_args))
    {
        throw new ArgumentException("gem: cannot execute without arguments.", "gem_args");
    }

    var gem_cmd = Build.GetPath("gem");

    if (!gem_cmd.Global)
    {
        Build.Log.Warn("gem: is not available on the system -- the command will not be executed.");
    }

    // trim the arguments
    gem_args = gem_args.Trim();
    gem_options = gem_options.Trim();

    Build.Log.Argument("cli", gem_cmd.Path, gem_cmd.Global);
    Build.Log.Argument("arguments", gem_args);
    Build.Log.Argument("options", gem_options);
    Build.Log.Argument("path", gem_path);
    Build.Log.Argument("wait", gem_wait);
    Build.Log.Argument("quiet", gem_quiet);
    Build.Log.Header();
}

exec exec_cmd='${ gem_cmd.Path }' exec_args='${ gem_args } ${ gem_options }' exec_path='${ gem_path }' exec_quiet='${ gem_quiet }' exec_redirect='${ false }' if='gem_cmd.Global'