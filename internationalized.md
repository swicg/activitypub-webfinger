# Internationalized Webfinger Handle Adoption

This document tracks adoption of non-ASCII characters in Webfinger handles in ActivityPub.

## Background

Most [ActivityPub](https://activitypub.rocks/) servers implement [Webfinger](https://datatracker.ietf.org/doc/html/rfc7033) for providing readable `localpart@domain.example` account identifiers. The [ActivityPub and Webfinger Report](https://www.w3.org/community/reports/socialcg/CG-FINAL-apwf-20240608/) outlines how Webfinger is implemented in the ActivityPub network.

For the top human languages, only 3 primarily use only ASCII characters (English, Malaysian, Indonesian), for a total of about 500 million people. That means about 90% of humans use languages with non-Latin, or accented, character sets.

Webfinger supports non-ASCII characters in both the local part (user name) and domain. In this document, we call Webfinger handles with non-ASCII characters **internationalized Webfinger handles**, **inclusive handles**, or **non-ASCII handles**. This document tracks adoption of non-ASCII characters in Webfinger handles in ActivityPub.

## Implementation notes

Webfinger is defined in [RFC 7033](https://www.rfc-editor.org/rfc/rfc7033.html). It uses the `acct:` URI format, among others, defined in [RFC 7565](https://www.rfc-editor.org/rfc/rfc7033.html).

An `acct:` URI has the form `userpart@host`. Per the [internationalization considerations](https://www.rfc-editor.org/info/rfc7565/#section-6), these parts have the following restrictions.

### userpart

`userpart` must match the [PRECIS IdentifierClass](https://www.rfc-editor.org/info/rfc8264/#section-4.2). This includes "letters", "numbers", and a few ASCII punctuation marks.

Some example userpart values:

- `user1`
- `renée`
- `иван`
- `ελπίδα`
- `小明`
- `किरण`
- `ليلى`
- `שרה`
- `anne.o'neill+notes`
- `민수`
- `さくら`

Not every implementation will support every form of userpart described here.

### host

`host` must match the Internationalized Domain Names for Applications (IDNA) requirements for the Unicode form of a hostname in [RFC 5892](https://www.rfc-editor.org/info/rfc5892/).

Some example host values:

- `ตัวอย่าง.example`
- `উদাহরণ.example`
- `எடுத்துக்காட்டு.example`
- `ఉదాహరణ.example`
- `მაგალითი.example`
- `example.հայ`
- `ምሳሌ.example`
- `example.net`
- `example.مصر`
- `example.中国`
- `café.example`

### acct: URI

In `acct:` URIs, the `userpart` is percent-encoded, and the `host` is encoded per IDNA.

Some examples:

- `acct:user1@xn--72c1a1bt4awk9o.example`
- `acct:ren%C3%A9e@xn--d5b6ci4b4b3a.example`
- `acct:%D0%B8%D0%B2%D0%B0%D0%BD@xn--4kcoa2bca5aa0vzacf2mdd.example`
- `acct:%CE%B5%CE%BB%CF%80%CE%AF%CE%B4%CE%B1@xn--noc6ci4b4b3a.example`
- `acct:%E5%B0%8F%E6%98%8E@xn--lodafveble.example`
- `acct:%E0%A4%95%E0%A4%BF%E0%A4%B0%E0%A4%A3@example.xn--y9a3aq`
- `acct:%D9%84%D9%8A%D9%84%D9%89@xn--mxd7a1d.example`
- `acct:%D7%A9%D7%A8%D7%94@example.net`
- `acct:anne.o'neill+notes@example.xn--wgbh1c`
- `acct:%EB%AF%BC%EC%88%98@example.xn--fiqs8s`
- `acct:%E3%81%95%E3%81%8F%E3%82%89@xn--caf-dma.example`

### Webfinger handle lookup

Looking up a handle requires converting it to an `acct:` URI and passing it as the `resource` parameter to the well-known URL for Webfinger for the host domain.

As an example, start with the handle `ελπίδα@ఉదాహరణ.example`. Its hostname in IDNA A-label form is `xn--noc6ci4b4b3a.example`. The `acct:` URI is:

```uri
acct:%CE%B5%CE%BB%CF%80%CE%AF%CE%B4%CE%B1@xn--noc6ci4b4b3a.example
```

The resulting URI for WebFinger lookup is:

```url
https://xn--noc6ci4b4b3a.example/.well-known/webfinger?resource=acct%3A%25CE%25B5%25CE%25BB%25CF%2580%25CE%25AF%25CE%25B4%25CE%25B1%40xn--noc6ci4b4b3a.example
```

Note that the `%` characters in the `acct:` URI are themselves percent-encoded in the HTTPS URL, as parameter values.

### ActivityPub object IDs

ActivityPub objects, including actors, activities, and collections, use HTTPS URIs as identifiers. These should use the IDNA A-label for the host. For example:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://xn--d5b6ci4b4b3a.example/note/35",
  "type": "Note",
  "content": "Hello, World!"
}
```

Some ActivityPub implementations include the username in URLs. These should be percent-encoded in object IDs.

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://xn--4kcoa2bca5aa0vzacf2mdd.example/user/%D0%B8%D0%B2%D0%B0%D0%BD/followers",
  "type": "OrderedCollection",
  "summary": "Followers of иван@எடுத்துக்காட்டு.example"
}
```

### preferredUsername

The `preferredUsername` of an actor should match the unencoded version of the `userpart`.

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://xn--caf-dma.example/user/%E3%81%95%E3%81%8F%E3%82%89",
  "type": "Person",
  "preferredUsername": "さくら"
}
```

### webfinger property

[FEP 2c59](https://fediverse.codeberg.page/fep/fep/2c59/) defines a `webfinger` property for an actor. This should be in the unencoded format of both the userpart and the host:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://xn--mxd7a1d.example/user/D9%8A%D9%84%D9%89",
  "type": "Person",
  "webfinger": "ليلى@ምሳሌ.example"
}
```

If the publisher uses the (less preferred) `acct:` URI format, it should be encoded:

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "id": "https://xn--mxd7a1d.example/user/D9%8A%D9%84%D9%89",
  "type": "Person",
  "webfinger": "acct:%D9%84%D9%8A%D9%84%D9%89@xn--mxd7a1d.example"
}
```

## Server and client matrix

This matrix is for tracking ActivityPub software implementation status of non-ASCII Webfinger handles.

It includes ActivityPub servers, ActivityPub server frameworks, Mastodon API clients, and ActivityPub API clients.

The table has the following columns:

- Software: name and link to the software.
- Receive: Can local users receive activities from a remote account with a non-ASCII handle?
- Send: Can local users send activities to a remote account with a non-ASCII handle?
- Link: Are non-ASCII handles linkified in in-band mentions?
- Search: Can the server's search interface discover an actor with a non-ASCII handle?
- Domain: Can a server be hosted on a domain with non-ASCII characters?
- Username: Can users on the server have a non-ASCII localpart (username) in their handle?
- Issue(s): Task tracking for these or related features

Thanks to [FediDB](https://fedidb.com/software) for the seed version of the software list.

| Software | Receive | Send | Link | Search | Domain | Username | Issue(s) |
| -------- | ------- | ---- | ---- | ------ | ------ | -------- | -------- |
| [Activity-Relay](https://relay.toot.yukimochi.jp/) | ? | ? | ? | ? | ? | ? | ? |
| [activitypub.bot](https://github.com/evanp/activitypub-bot)[^1] | ✅ Y | ✅ Y | ✅ Y | ✅ Y | ✅ Y | ✅ Y | [#282](https://github.com/evanp/activitypub-bot/issues/282) |
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
| [Forte](https://codeberg.org/fortified/forte) | ✅ Y | ✅ Y | ✅ Y | ✅ Y | ✅ Y | ✅ Y | [^2] |
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
| [Manyfold](https://manyfold.app/) | ? | ? | ? | ? | ? | ? | [#7310](https://github.com/manyfold3d/manyfold/issues/7310) + Fedipub support |
| [Mastodon](https://joinmastodon.org/) | ? | ? | ? | ? | ? | ? | [#8417](https://github.com/mastodon/mastodon/issues/8417) |
| [Mbin](https://joinmbin.org/) | ? | ? | ? | ? | ? | ? | ? |
| [Meisskey](https://github.com/mei23/misskey) | ? | ? | ? | ? | ? | ? | ? |
| [Microblogpub](https://microblog.pub/) | ? | ? | ? | ? | ? | ? | ? |
| [Microdotblog](https://micro.blog/) | ? | ? | ? | ? | ? | ? | ? |
| [Misskey](https://misskey-hub.net/) | ? | ? | ? | ? | ? | ? | ? |
| [Mitra](https://codeberg.org/silverpill/mitra) | ? | ? | ? | ? | ? | ? | ? |
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
| [streams repository](https://codeberg.org/streams/streams) | ✅ Y | ✅ Y | ✅ Y | ✅ Y | ✅ Y | ✅ Y | [^2] |
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

- Library: name and link to the library.
- Forward: Can the library discover an ActivityPub actor from a non-ASCII handle?
- Reverse: Can the library construct a non-ASCII handle from an ActivityPub actor?
- Issue(s): Task tracking for these or related features

| Software | Forward | Reverse | Issue(s) |
| -------- | ------- | ------- | -------- |
| [activitypub-webfinger](https://github.com/social-web-foundation/activitypub-webfinger) | ✅ Y [^3] | ✅ Y [^3] | [#1](https://github.com/social-web-foundation/activitypub-webfinger/issues/1) |
| [Fedipub](https://fedipub.dev) | ? | ? | [#62](https://gitlab.com/fedipub/fedipub/-/work_items/62) |
| [webfinger.js](https://github.com/silverbucket/webfinger.js) | ✅ Y [^4] | N/A | [#179](https://github.com/silverbucket/webfinger.js/issues/179) |

[^3]: From version 0.3.0
[^4]: v3.1.0+

## Submitting data

To share data about new ActivityPub and Webfinger implementations and their support for non-ASCII Webfinger handles, make a new pull request to <https://github.com/swicg/activitypub-webfinger/>.
