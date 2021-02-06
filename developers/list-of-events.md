---
title: List of Events
description: A list of events available for event listeners through the EventDispatcher
published: true
date: 2021-02-06T20:26:47.046Z
tags: 
editor: markdown
dateCreated: 2021-02-06T20:12:02.133Z
---

# `\App\Event\BuildAdminMenu`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/BuildAdminMenu.php)

This event allows you to customize and extend the Administration page menu.

```php
$dispatcher->addListener(Event\BuildAdminMenu::class, function(Event\BuildAdminMenu $event) {
    $router = $event->getRouter();

    $event->addItem('example', [
        'label' => __('Example'),
        'icon' => 'cast',
        'url' => $router->fromHere('example-plugin:admin:example'),
        'permission' => Acl::GLOBAL_VIEW,
    ]);
});
```

# `\App\Event\BuildConsoleCommands`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/BuildConsoleCommands.php)

This event allows you to register your own CLI console commands, which appear when running the [AzuraCast CLI](http://www.azuracast.com/cli.html).

```php
$dispatcher->addListener(Event\BuildConsoleCommands::class, function (Event\BuildConsoleCommands $event) {
    $console = $event->getConsole();

    $console->command(
        'plugin:example',
        Plugin\ExamplePlugin\Command\ExampleCommand::class,
    )->setDescription('Execute the example command');
}, -1);
```

# `\App\Event\BuildDoctrineMappingPaths`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/BuildDoctrineMappingPaths.php)

This event allows you to register custom doctrine mappings for your own database entities.

```php
$dispatcher->addListener(Event\BuildDoctrineMappingPaths::class, function (Event\BuildDoctrineMappingPaths $event) {
    $mappingClassesPaths = $event->getMappingClassesPaths();
    $baseDir = $event->getBaseDir();

    $mappingClassesPaths[] = $baseDir . '/plugins/ExamplePlugin/src/Entity';

    $event->setMappingClassesPaths($mappingClassesPaths);
});
```

# `\App\Event\BuildRoutes`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/BuildRoutes.php)

This event allows you to register custom routes to the HTTP Router. This allows you to create entirely new routes handled exclusively by your plugins.

# `\App\Event\BuildView`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/BuildView.php)

This event lets you inject custom data into the template renderer, or modify the existing data that's already injected. This includes the current user, current station, page title, etc.

# `\App\Event\SendWebhooks`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/SendWebhooks.php)

This event is triggered any time web hooks are triggered for a station. It includes the current "now playing" data along with a list of the triggers that are associated with the webhook (i.e. if the song changed, DJ connected/disconnected, etc).

# `\App\Event\Radio\AnnotateNextSong`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/Radio/AnnotateNextSong.php)

This event is triggered once the next song has been determined by the AzuraCast AutoDJ software and is being sent to Liquidsoap. Annotations allow you to customize fade-in, fade-out, cue-in and cue-out data, and the artist/title displayed for a song.

# `\App\Event\Radio\GenerateRawNowPlaying`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/Radio/GenerateRawNowPlaying.php)

This event is triggered when building the "Now Playing" data for a given station. This data is called "raw" because it has not been converted yet into the standardized API response format served by AzuraCast's API. By modifying the "raw" nowplaying response, you can change the currently playing song or modify the listener count.

# `\App\Event\Radio\GetNextSong`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/Radio/GetNextSong.php)

This event is triggered as the AzuraCast AutoDJ is determining the next song to play for a given station. By default, ths checks for an existing "next song" record in the database, and if one isn't present, determines what should play next based on all available playlists and their scheduling status.

# `\App\Event\Radio\WriteLiquidsoapConfiguration`

- [Class reference](https://github.com/AzuraCast/AzuraCast/blob/master/src/Event/Radio/WriteLiquidsoapConfiguration.php)

This event is triggered when changes to the Liquidsoap configuration are being written to disk. This process uses the Event Dispatcher because it is by far the most complex configuration file written by the system, and there are multiple points at which one may want to override the configuration written by AzuraCast itself. Users are already able to write custom configuration to one specific location (between the playlists being built and mixed with the live signal, and the signal being broadcast to local and remote sources), but overriding this event allows you to modify the configuration in any other location.