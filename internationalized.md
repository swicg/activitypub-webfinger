# Internationalized Webfinger Handle Adoption

This document tracks adoption of non-ASCII characters in Webfinger handles in ActivityPub.

## Background

Most [ActivityPub](https://activitypub.rocks/) servers implement [Webfinger](https://datatracker.ietf.org/doc/html/rfc7033) for providing readable `localpart@domain.example` account identifiers. The [ActivityPub and Webfinger Report](https://www.w3.org/community/reports/socialcg/CG-FINAL-apwf-20240608/) outlines how Webfinger is implemented in the ActivityPub network.

For the top human languages, only 3 primarily use only ASCII characters (English, Malaysian, Indonesian), for a total of about 500 million people. That means about 90% of humans use languages with non-Latin, or accented, character sets.

Webfinger supports non-ASCII characters in both the local part (user name) and domain. In this document, we call Webfinger handles with non-ASCII characters **internationalized Webfinger handles**, **inclusive handles**, or **non-ASCII handles**. This document tracks adoption of non-ASCII characters in Webfinger handles in ActivityPub.

## Server and client matrix

This matrix is for tracking ActivityPub software implementation status of non-ASCII Webfinger handles.

It includes ActivityPub servers, ActivityPub server frameworks, Mastodon API clients, and ActivityPub API clients.

The table has the following columns:

* Software: name and link to the software.
* Receive: Can local users receive activities from a remote account with a non-ASCII handle?
* Send: Can local users send activities to a remote account with a non-ASCII handle?
* Link: Are non-ASCII handles linkified in in-band mentions?
* Search: Can the server's search interface discover an actor with a non-ASCII handle?
* Domain: Can a server be hosted on a domain with non-ASCII characters?
* Username: Can users on the server have a non-ASCII localpart (username) in their handle?
* Issue(s): Task tracking for these or related features

Thanks to [FediDB](https://fedidb.com/software) for the seed version of the software list.

| Software | Receive | Send | Link | Search | Domain | Username | Issue(s) |
| -------- | ------- | ---- | ---- | ------ | ------ | -------- | -------- |
| [Activity-Relay](https://relay.toot.yukimochi.jp/) | ? | ? | ? | ? | ? | ? | ? |
| [activitypub.bot](https://github.com/evanp/activitypub-bot)[^1] | ? | ? | ? | ? | ? | ? | [#282](https://github.com/evanp/activitypub-bot/issues/282) |
| [Activityrelay](https://git.pleroma.social/pleroma/relay) | ? | ? | ? | ? | ? | ? | ? |
| [Akkoma](https://akkoma.dev/AkkomaGang/akkoma) | ? | ? | ? | ? | ? | ? | ? |
| [BadgeFed](https://badgefed.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Betula](https://betula.mycorrhiza.wiki/) | ? | ? | ? | ? | ? | ? | ? |
| [Bonfire](https://bonfirenetworks.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Bookwyrm](https://joinbookwyrm.com/) | ? | ? | ? | ? | ? | ? | ? |
| [Bridgy-fed](https://fed.brid.gy/) | ? | ? | ? | ? | ? | ? | ? |
| [browser.pub](https://browser.pub/) | ? | ? | ? | ? | ? | ? | ? |
| [Cherrypick](https://github.com/kokonect-link/cherrypick) | ? | ? | ? | ? | ? | ? | ? |
| [Drupal](https://www.drupal.org/project/activitypub) | ? | ? | ? | ? | ? | ? | ? |
| [Ecko](https://magicstone.dev/) | ? | ? | ? | ? | ? | ? | ? |
| [Emissary](https://emissary.dev/) | ? | ? | ? | ? | ? | ? | ? |
| [Fedibird](https://github.com/fedibird/mastodon) | ? | ? | ? | ? | ? | ? | ? |
| [Firefish](https://fedidb.com/software/firefish) | ? | ? | ? | ? | ? | ? | ? |
| [Forgejo](https://fedidb.com/software/forgejo) | ? | ? | ? | ? | ? | ? | ? |
| [Forte](https://codeberg.org/fortified/forte) | Y | Y | Y | Y | Y | Y | [^2] |
| [Foundkey](https://akkoma.dev/FoundKeyGang/FoundKey) | ? | ? | ? | ? | ? | ? | ? |
| [frequency](https://frequency.app/) | ? | ? | ? | ? | ? | ? | ? |
| [Friendica](https://friendi.ca/) | ? | ? | ? | ? | ? | ? | ? |
| [Funkwhale](https://funkwhale.audio/) | ? | ? | ? | ? | ? | ? | ? |
| [Gancio](https://gancio.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Ghost](https://ghost.org/) | ? | ? | ? | ? | ? | ? | ? |
| [GNU social](https://gnusocial.network/) | ? | ? | ? | ? | ? | ? | ? |
| [Gotosocial](https://gotosocial.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Hackers' Pub](https://hackers.pub/) | ? | ? | ? | ? | ? | ? | ? |
| [Hollo](https://docs.hollo.social/) | ? | ? | ? | ? | ? | ? | ? |
| [Hometown](https://github.com/hometown-fork/hometown) | ? | ? | ? | ? | ? | ? | ? |
| [Honk](https://fedidb.com/software/honk) | ? | ? | ? | ? | ? | ? | ? |
| [Hubzilla](https://hubzilla.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Iceshrimp](https://iceshrimp.dev/iceshrimp/iceshrimp) | ? | ? | ? | ? | ? | ? | ? |
| [irwin](https://otisburg.social/) | ? | ? | ? | ? | ? | ? | ? |
| [Kmyblue](https://github.com/kmycode/mastodon) | ? | ? | ? | ? | ? | ? | ? |
| [Ktistec](https://github.com/toddsundsted/ktistec) | ? | ? | ? | ? | ? | ? | ? |
| [Lemmy](https://join-lemmy.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Loops](https://joinloops.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Lotide](https://git.sr.ht/~vpzom/lotide) | ? | ? | ? | ? | ? | ? | ? |
| [Manyfold](https://manyfold.app/) | ? | ? | ? | ? | ? | ? | ? |
| [Mastodon](https://joinmastodon.org/) | ? | ? | ? | ? | ? | ? | [#8417](https://github.com/mastodon/mastodon/issues/8417) |
| [Mbin](https://joinmbin.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Meisskey](https://github.com/mei23/misskey) | ? | ? | ? | ? | ? | ? | ? |
| [Microblogpub](https://microblog.pub/) | ? | ? | ? | ? | ? | ? | ? |
| [Microdotblog](https://micro.blog/) | ? | ? | ? | ? | ? | ? | ? |
| [Misskey](https://misskey-hub.net/) | ? | ? | ? | ? | ? | ? | ? |
| [Mitra](https://codeberg.org/silverpill/mitra) | ? |? | ? | ? | ? | ? | ? |
| [Mobilizon](https://joinmobilizon.org/) | ? | ? | ? | ? | ? | ? | ? |
| [murlog](https://murlog.org/) | ? | ? | ? | ? | ? | ? | ? |
| [NeoDB](https://neodb.net/) | ? | ? | ? | ? | ? | ? | ? |
| [NodeBB](https://nodebb.org/) | ? | ? | ? | ? | ? | ? | ? |
| [onepage.pub](https://github.com/evanp/onepage.pub/) | ? | ? | ? | ? | ? | ? | [#287](https://github.com/evanp/onepage.pub/issues/287) |
| [Owncast](https://owncast.online/) | ? | ? | ? | ? | ? | ? | ? |
| [PeerTube](https://joinpeertube.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Piefed](https://join.piefed.social/) | ? | ? | ? | ? | ? | ? | ? |
| [Pixelfed](https://pixelfed.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Pleroma](https://pleroma.social/) | ? | ? | ? | ? | ? | ? | ? |
| [Plume](https://joinplu.me/) | ? | ? | ? | ? | ? | ? | ? |
| [Postmarks](https://postmarks.glitch.me/) | ? | ? | ? | ? | ? | ? | ? |
| [Sharkey](https://joinsharkey.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Smithereen](https://smithereen.software/) | ? | ? | ? | ? | ? | ? | ? |
| [Snac](https://codeberg.org/grunfink/snac2) | ? | ? | ? | ? | ? | ? | ? |
| [stegodon](https://stegodon.social/) | ? | ? | ? | ? | ? | ? | ? |
| [streams repository](https://codeberg.org/streams/streams) | Y | Y | Y | Y | Y | Y | [^2] |
| [Takahe](https://jointakahe.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Threads](https://threads.net/) | ? | ? | ? | ? | ? | ? | ? |
| [tootik](https://github.com/dimkr/tootik) | ? | ? | ? | ? | ? | ? | ? |
| [Vernissage](https://vernissage.photos/home) | ? | ? | ? | ? | ? | ? | ? |
| [wafrn](https://wafrn.net/) | ? | ? | ? | ? | ? | ? | ? |
| [Webfinger Browser](https://github.com/social-web-foundation/acct-handler) | N/A | N/A | N/A | ✅ Y | N/A | N/A | [#35](https://github.com/social-web-foundation/acct-handler/issues/35) |
| [Welley](https://welley.social/) | ? | ? | ? | ? | ? | ? | ? |
| [WordPress](https://wordpress.org/) | ? | ? | ? | ? | ? | ? | ? |
| [WriteFreely](https://writefreely.org/) | ? | ? | ? | ? | ? | ? | ? |

[^1]: [tags.pub](https://tags.pub/), [groups.pub](https://groups.pub/), [activitypub.bot](https://activitypub.bot/)
[^2]: IDN (punycode) encoded for federation and translated internally for display. Uses 'petnames' for easier mentioning and searching.

## Library matrix

This matrix is for tracking ActivityPub software library support for non-ASCII Webfinger handles.

The table has the following columns:

* Library: name and link to the library.
* Forward: Can the library discover an ActivityPub actor from a non-ASCII handle?
* Reverse: Can the library construct an ActivityPub actor from a non-ASCII handle?
* Issue(s): Task tracking for these or related features

| Software | Forward | Reverse | Issue(s) |
| -------- | ------- | ------- | -------- |
| [activitypub-webfinger](https://github.com/social-web-foundation/activitypub-webfinger) | ? | ? | [#1](https://github.com/social-web-foundation/activitypub-webfinger/issues/1) |
| [webfinger.js](https://github.com/silverbucket/webfinger.js) | ✅ Y (v3.1.0+) | N/A | [#179](https://github.com/silverbucket/webfinger.js/issues/179) |

## Submitting data

To share data about new ActivityPub and Webfinger implementations and their support for non-ASCII Webfinger handles, make a new pull request to <https://github.com/swicg/activitypub-webfinger/>.
