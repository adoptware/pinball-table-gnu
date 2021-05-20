# PINBALL-TABLE-GNU #

[![GitHub forks](
https://img.shields.io/github/forks/rzr/pinball-table-gnu.svg?style=social&label=Fork&maxAge=2592000
)](
https://github.com/rzr/pinball-table-gnu
)
[![Packaging status](
https://repology.org/badge/tiny-repos/pinball-table-gnu.svg
)](
https://repology.org/project/pinball-table-gnu/versions
)
[![Fediverse](
https://img.shields.io/mastodon/follow/279303?domain=https%3A%2F%2Fmastodon.social&style=social#
)](
https://mastodon.social/@rzr/103765397111911239#pinball-table-gnu#
)
[![snap](
https://snapcraft.io/pinball-table-gnu/badge.svg
)](
https://snapcraft.io/pinball-table-gnu
)

## INFOS ##

GNU Pinball table for emilia pinball

Another pinball table to have fun, behaviour plugin.
The table is dedicated to GNU project.
It features magnets and bumpers.

For more details check file:

- [data/gnu/README](data/gnu/README)

[![Get it from the Snap Store](
https://snapcraft.io/static/images/badges/en/snap-store-black.svg
)](
https://snapcraft.io/pinball-table-gnu
)

## DEMO ##

[![Video](
https://files.mastodon.social/cache/preview_cards/images/023/049/736/original/ec1762c91f84251b.jpg
)](
https://rzr.github.io/rzr-presentations/docs/pinball/#/11/:PinballTableGnu:
)

Watch video on PeerTube:

- <https://diode.zone/videos/watch/ab13e09e-ffa2-41cc-bb7a-9f06d18e6d9c#pinball-table-gnu-pincab-2020-rzr>

## USAGE ##

It's easy build table assuming that Emilia pinball headers installed in system.

```sh
./bootstrap && ./configure && make && make install
```

For Debian users building a package can be automated using:

```sh
./debian/Makefile rule/setup && ./debian/Makefile && sudo debi
```

Then new table should be listed when you run pinball game.

Also a Dockerfile can be used as reference env.

```sh
docker-compose up --build
```

## RESOURCES ##

- <https://repology.org/project/pinball-table-gnu>
- <https://tracker.debian.org/pkg/pinball-table-gnu>
- <https://pinball.sf.net>
- <https://purl.org/rzr/pinball>
- <https://github.com/rzr/pinball/issues/4>
- <https://mastodon.social/@rzr/103448015175182101>
- <https://rzr.github.io/rzr-presentations/docs/pinball/>
- <https://purl.org/rzr>
- <https://purl.org/rzr/presentations>
- <https://mdco2.mini.debconf.org/talks/23-my-diy-pinball-on-debian/>
- <https://peertube.debian.social/videos/watch/c23f0709-4099-4302-b877-f7d2562b2880#my-diy-pinball-on-debian>
- <https://github.com/rzr/pinball-table-hurd/>
