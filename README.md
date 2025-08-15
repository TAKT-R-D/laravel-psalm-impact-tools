# laravel-psalm-impact-tools

A collection of shell scripts for advanced static analysis and impact/reference tracing in Laravel PHP projects using Psalm.

## Overview

This toolkit provides:

-   **impact**: Multi-level impact analysis (BFS) for classes/files, showing what references a given class or file, and their transitive dependencies.
-   **impact-diff**: Runs impact analysis for all changed PHP files vs a base git ref.
-   **psref**: Simple reference finder for a class or file to see details.
-   **psref-diff**: Runs reference finding for all changed PHP files vs a base git ref.
-   **psalm**: Wrapper for running Psalm static analysis with recommended options.

## Set up

### Install psalm and laravel plugin

```
$ composer require --dev 'vimeo/psalm:^6.13' 'psalm/plugin-laravel:^3.0'
$ mkdir -p .psalm-cache
```

### Configure psalm

```xml:/psalm.xml
<?xml version="1.0"?>
<psalm
    errorLevel="3"
    resolveFromConfigFile="true"
    findUnusedCode="true"
    cacheDirectory="./.psalm-cache"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="vendor/vimeo/psalm/config.xsd"
>
  <projectFiles>
    <directory name="app" />
    <directory name="routes" />
    <directory name="database" />
    <ignoreFiles>
      <directory name="vendor" />
      <directory name="storage" />
      <directory name="bootstrap/cache" />
      <directory name="node_modules" />
    </ignoreFiles>
  </projectFiles>
  <plugins>
    <pluginClass class="Psalm\LaravelPlugin\Plugin" />
  </plugins>
</psalm>
```

### Place toolkit

Place all scripts under `<your laravel project root>/bin`.
`chmod +x` to grant executable permission.

## Usage

Place toolkit under `<your project root>/bin`.
All scripts are intended to be run from the project root. See each script's header for details and examples.

### Example: Impact Analysis

```sh
bin/impact app/Services/WeatherService.php
bin/impact App\\Services\\WeatherService 4
```

### Example: Reference Finding

```sh
bin/psref app/Services/WeatherService.php
bin/psref App\\Services\\WeatherService
```

### Example: Diff-based Analysis

```sh
bin/impact-diff
bin/psref-diff remotes/origin/main
```

### Example: Psalm Static Analysis

```sh
bin/psalm
bin/psalm --show-info=true
```

## Requirements

-   zsh
-   Psalm (installed via Composer)
-   Laravel project structure

## License

MIT
