# kiwi-catalog

The app catalog for the [Kiwi Network](https://kiwi-network.eu), consumed by
[kiwi-updater](https://github.com/derlocke-ng/kiwi-updater). Register it once:

```bash
kiwi catalog add https://github.com/derlocke-ng/kiwi-catalog.git
```

`kiwi` syncs this repo before every check and update, so managing a fleet is
just git: **add an app** = add its URL to [apps.list](apps.list) and commit;
**pin or roll back** = append `ref=<tag>`; **remove** = delete the line
(installed copies stay until uninstalled). A machine's own `apps.list` always
wins over a catalog.

## One line per app

An app that installs both a root service and a desktop component is listed
**once**. Its `kiwi.manifest` declares `SCOPES=user system` and `kiwi` installs
each half correctly — the user half from `~/.local`, the system half cloned and
run as root, so root never executes a file the user can write.

`scope=` on a catalog line is a *filter* for machines that should only get one
half, not a declaration.

## Adding your app

Put a [`kiwi.manifest`](https://github.com/derlocke-ng/kiwi-updater/blob/main/templates/kiwi.manifest)
and an `install.sh` in your repo root, tag a release, then open a PR adding the
URL here. Anything open source and genuinely usable without an account, a
subscription or a tracker is welcome.

If your app needs root, say why in `ROOT_REASON=` — `kiwi` shows that sentence
to the user before it asks for a password.

## Trust

Installing an app runs that repo's `install.sh`, and an app declaring the system
scope runs it **as root**. This catalog is curated, not audited — being listed
here means someone thought the app was worth having, not that its code has been
reviewed. Read `kiwi info <app>` before installing something you do not already
know.

Pinning, update diffs and signature checking are
[planned in kiwi-updater](https://github.com/derlocke-ng/kiwi-updater#planned-choosing-what-you-run).
Until then the honest position is: adding a catalog is a decision about whose
code you are willing to run, and `ref=<tag>` on an entry is how you stop it
changing underneath you.
