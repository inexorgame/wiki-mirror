### Media Repository

Why `media repositories`?

While sauerbraten was packaged as game, new content was provided with new releases only. Inexor will change this. New content will be available faster.

A media repository contains maps, textures, sounds and so on (`media items`). Inexor can handle multiple repositories at the same time. The repositories can be local or remote and will be cached locally. Every user can provide their repositories publicly. The Inexor Team will provide a `core repository` with entirely free content itself.

The basics:

Every media repository has a name, a description and an URL where it is located. There are multiple types of media repositories:

* Local filesystem
* Git repository
* (future) Database

Inexor automatically scans the file system for media repositories in the usual locations depending on the operating system (XDG paths). For example Ubuntu Linux Inexor scans in these directories:

* /home/noob/.local/share/inexor/media
* /usr/share/ubuntu/inexor/media
* /usr/share/gnome/inexor/media
* /usr/local/share/inexor/media
* /usr/share/inexor/media
* /var/lib/snapd/desktop/inexor/media

### Media Items

Each `media repository` contains `media items`. The file system structure is clearly defined to enable automatic indexation.

<pre>
/[repository_name]/[media_type]/[author_name/author_group_name]/[media_name]/v[version]/
</pre>

For example the path to a texture named golden_floor from noob in the repository noob is located here:

<pre>
/core/texture/noob/golden_floor/v1/
</pre>

The folder contains the texture files and a configuration file:

<pre>
ls /core/texture/noob/golden_floor/v1/
  diffuse.jpg
  heightmap.jpg
  normalmap.jpg
  config.toml (or config.json)
</pre>

The path contains the version number. This way a media repository can contain multiple versions of a media item at the same time. For example the map dust6 depends on version 2 of texture golden_floor whereas map cartel depends on version 8 of the same texture.

#### Media Types

A `media item` is one these `media types`:

* brush
* map
* model
* music
* particle
* skybox
* sound
* texture

#### Media Item Configuration

Either a TOML or a JSON file specifies the meta information of a `media item`.

<pre>
{
  "name": "dust6",
  "description": "Map A",
  "license": "cc4.0by",
  "authors": [
    {
      "name": "noob",
      "email": "info@inexor.org"
    }
  ],
  "dependencies": {
    "textures": [
      {
          "repository": "https://github.com/inexor-game/data.git",
          "name": "golden_floor",
          "media_type": "texture",
          "version": 1
      }
    ]
  }
}
</pre>

The dependencies contains the repository where to get the media item and the name and version of the media item. These informations are enough to resolve a media item.

(future) If a media repository is missing, Inexor suggest the user to add the media repository.