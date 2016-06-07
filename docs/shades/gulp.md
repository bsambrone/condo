---
layout: docs
title: gulp
group: shades
---

@{/*

gulp
    Executes a gulp task runner command.

gulp_args=''
    Required. The arguments to pass to the gulp command line tool.

gulp_options='' (Environment Variable: GULP_OPTIONS)
    Additional options to pass to the gulp command.

gulp_path='$(base_path)'
    The path in which to execute gulp.

base_path='$(CurrentDirectory)'
    The base path in which to execute gulp.

working_path='$(base_path)'
    The working path in which to execute gulp.

gulp_wait='true'
    A value indicating whether or not to wait for exit.

gulp_quiet='$(Build.Log.Quiet)'
    A value indicating whether or not to avoid printing output.

*/}

use namespace = 'System'
use namespace = 'System.IO'
use namespace = 'System.Linq'

use import = 'Condo.Build'

default base_path           = '${ Directory.GetCurrentDirectory() }'
default working_path        = '${ base_path }'

default gulp_args           = ''
default gulp_options        = '${ Build.Get("GULP_OPTIONS") }'
default gulp_path           = '${ working_path }'
default gulp_wait           = '${ true }'
default gulp_quiet          = '${ Build.Log.Quiet }'

gulp-download once='gulp-download'

@{
    Build.Log.Header("gulp");

    var gulp_cmd = Build.GetPath("gulp");

    if (!string.IsNullOrEmpty(gulp_args))
    {
        gulp_args = gulp_args.Trim();
    }

    if (!string.IsNullOrEmpty(gulp_options))
    {
        gulp_options = gulp_options.Trim();
    }

    Build.Log.Argument("arguments", gulp_args);
    Build.Log.Argument("options", gulp_options);
    Build.Log.Argument("path", gulp_path);
    Build.Log.Argument("wait", gulp_wait);
    Build.Log.Argument("quiet", gulp_quiet);
    Build.Log.Header();
}

exec exec_cmd='${ gulp_cmd.Path }' exec_args='${ gulp_args } ${ gulp_options }' exec_path='${ gulp_path }' exec_wait='${ gulp_wait }' exec_quiet='${ gulp_quiet }' exec_redirect='${ false }' if='gulp_cmd.Global'
node node_args='"${ gulp_cmd.Path }" ${ gulp_args } ${ gulp_options }' node_path='${ gulp_path }' node_wait='${ gulp_wait }' node_quiet='${ gulp_quiet }' if='!gulp_cmd.Global'