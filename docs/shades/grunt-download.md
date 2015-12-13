---
layout: docs
title: grunt-download
group: shades
---

@{/*

grunt-download
    Downloads and installs grunt if it is not already installed.

grunt_download_path='$(base_path)'
    The path in which to download grunt.

base_path='$(CurrentDirectory)'
    The base path in which to download grunt.

*/}

use import = 'Condo.Build'

default base_path                   = '${ Directory.GetCurrentDirectory() }'

default grunt_download_path         = '${ base_path }'

@{
    Build.Log.Header("grunt-download");

    var grunt_install               = Path.Combine(grunt_download_path, "node_modules", "grunt-cli", "bin", "grunt");
    var grunt_locally_installed     = File.Exists(grunt_install);
    var grunt_version               = string.Empty;
    var grunt_globally_installed    = !grunt_locally_installed && Build.TryExecute("grunt", out grunt_version, "--version --config.interactive=false");
    var grunt_installed             = grunt_locally_installed || grunt_globally_installed;

    grunt_install = grunt_globally_installed ? "grunt" : grunt_install;

    Build.Log.Argument("path", grunt_install);
    Build.Log.Header();

    if (grunt_globally_installed)
    {
        Build.Log.Argument("version", grunt_version);
    }
}

npm-install npm_install_id='grunt' npm_install_prefix='${ grunt_download_path }' if='!grunt_installed'

- Build.SetPath("grunt", grunt_install, grunt_globally_installed);